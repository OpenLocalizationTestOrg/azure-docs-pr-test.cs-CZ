---
title: "aaaTroubleshoot odstraňování účtů úložiště Azure, kontejnery nebo virtuální pevné disky v nasazení classic | Microsoft Docs"
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
ms.openlocfilehash: 6bbfa032e1968718c623227bb426d553e2951075
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deleting-azure-storage-accounts-containers-or-vhds-in-a-classic-deployment"></a><span data-ttu-id="61011-103">Řešení potíží s odstraňování účtů úložiště Azure, kontejnery nebo virtuální pevné disky v nasazení classic</span><span class="sxs-lookup"><span data-stu-id="61011-103">Troubleshoot deleting Azure storage accounts, containers, or VHDs in a classic deployment</span></span>
[!INCLUDE [storage-selector-cannot-delete-storage-account-container-vhd](../../includes/storage-selector-cannot-delete-storage-account-container-vhd.md)]

<span data-ttu-id="61011-104">Taky může docházet k chybám při pokusu o účtu úložiště Azure hello toodelete, kontejneru nebo virtuální pevný disk v hello [portál Azure](https://portal.azure.com/) nebo hello [portál Azure classic](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="61011-104">You might receive errors when you try toodelete hello Azure storage account, container, or VHD in hello [Azure portal](https://portal.azure.com/) or hello [Azure classic portal](https://manage.windowsazure.com/).</span></span> <span data-ttu-id="61011-105">Hello problémů může být způsobeno hello následující okolnosti:</span><span class="sxs-lookup"><span data-stu-id="61011-105">hello issues can be caused by hello following circumstances:</span></span>

* <span data-ttu-id="61011-106">Při odstranění virtuálního počítače hello disku a virtuálního pevného disku nejsou automaticky odstraněny.</span><span class="sxs-lookup"><span data-stu-id="61011-106">When you delete a VM, hello disk and VHD are not automatically deleted.</span></span> <span data-ttu-id="61011-107">Který může být hello důvod selhání v odstranění účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="61011-107">That might be hello reason for failure on storage account deletion.</span></span> <span data-ttu-id="61011-108">Neodstraňujte jsme hello disku tak, aby můžete hello disku toomount jiným virtuálním Počítačem.</span><span class="sxs-lookup"><span data-stu-id="61011-108">We don't delete hello disk so that you can use hello disk toomount another VM.</span></span>
* <span data-ttu-id="61011-109">Na disku nebo hello objektů blob, který je spojen s hello disk stále není zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="61011-109">There is still a lease on a disk or hello blob that's associated with hello disk.</span></span>
* <span data-ttu-id="61011-110">Je stále image virtuálního počítače, který používá účet úložiště, kontejner nebo objekt blob.</span><span class="sxs-lookup"><span data-stu-id="61011-110">There is still a VM image that is using a blob, container, or storage account.</span></span>

<span data-ttu-id="61011-111">Pokud není Azure problém řešený v tomto článku, navštivte hello fóra Azure na [MSDN a hello Stack Overflow](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="61011-111">If your Azure issue is not addressed in this article, visit hello Azure forums on [MSDN and hello Stack Overflow](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="61011-112">Problém můžete zveřejnit na tyto fóra nebo too@AzureSupport na Twitteru.</span><span class="sxs-lookup"><span data-stu-id="61011-112">You can post your issue on these forums or too@AzureSupport on Twitter.</span></span> <span data-ttu-id="61011-113">Navíc můžete soubor žádost o podporu Azure tak, že vyberete **získat podporu** na hello [podporu Azure](https://azure.microsoft.com/support/options/) lokality.</span><span class="sxs-lookup"><span data-stu-id="61011-113">Also, you can file an Azure support request by selecting **Get support** on hello [Azure support](https://azure.microsoft.com/support/options/) site.</span></span>

## <a name="symptoms"></a><span data-ttu-id="61011-114">Příznaky</span><span class="sxs-lookup"><span data-stu-id="61011-114">Symptoms</span></span>
<span data-ttu-id="61011-115">Hello následující části jsou uvedeny běžné chyby, které se může zobrazit při pokusu o účtech Azure storage hello toodelete, kontejnery nebo virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="61011-115">hello following section lists common errors that you might receive when you try toodelete hello Azure storage accounts, containers, or VHDs.</span></span>

### <a name="scenario-1-unable-toodelete-a-storage-account"></a><span data-ttu-id="61011-116">Scénář 1: Nelze toodelete účet úložiště</span><span class="sxs-lookup"><span data-stu-id="61011-116">Scenario 1: Unable toodelete a storage account</span></span>
<span data-ttu-id="61011-117">Když přejdete účet úložiště classic toohello hello [portál Azure](https://portal.azure.com/) a vyberte **odstranit**, může být zobrazí se seznam objektů, které brání odstranění účtu úložiště hello:</span><span class="sxs-lookup"><span data-stu-id="61011-117">When you navigate toohello classic storage account in hello [Azure portal](https://portal.azure.com/) and select **Delete**, you may be presented with a list of objects that are preventing deletion of hello storage account:</span></span>

  ![Obrázek chyby při odstranění účtu úložiště hello](./media/storage-cannot-delete-storage-account-container-vhd/newerror.png)

<span data-ttu-id="61011-119">Když přejdete toohello účet úložiště v hello [portál Azure classic](https://manage.windowsazure.com/) a vyberte **odstranit**, můžete sledovat některé hello následujícím chybám:</span><span class="sxs-lookup"><span data-stu-id="61011-119">When you navigate toohello storage account in hello [Azure classic portal](https://manage.windowsazure.com/) and select **Delete**, you might see one of hello following errors:</span></span>

- <span data-ttu-id="61011-120">*Účet úložiště StorageAccountName obsahuje Image virtuálních počítačů. Zkontrolujte, zda že jsou tyto Image virtuálních počítačů odebrány před odstraněním tohoto účtu úložiště.*</span><span class="sxs-lookup"><span data-stu-id="61011-120">*Storage account StorageAccountName contains VM Images. Ensure these VM Images are removed before deleting this storage account.*</span></span>

- <span data-ttu-id="61011-121">*Nepodařilo účet úložiště toodelete < vm-storage účet name >. Účet úložiště nejde toodelete < vm-storage účet name >: "účet úložiště < vm-storage účet name > má některé aktivní Image a/nebo disky. Tyto bitové kopie a/nebo disky odebrání před odstraněním tohoto účtu úložiště.'.*</span><span class="sxs-lookup"><span data-stu-id="61011-121">*Failed toodelete storage account <vm-storage-account-name>. Unable toodelete storage account <vm-storage-account-name>: 'Storage account <vm-storage-account-name> has some active image(s) and/or disk(s). Ensure these image(s) and/or disk(s) are removed before deleting this storage account.'.*</span></span>

- <span data-ttu-id="61011-122">*Účet úložiště < vm-storage účet name > obsahuje některé aktivní Image a/nebo disky, například xxxxxxxxx-xxxxxxxxx-O-209490240936090599. Ujistěte se, tyto Image a/nebo disky jsou odebrány před odstraněním tohoto účtu úložiště.*</span><span class="sxs-lookup"><span data-stu-id="61011-122">*Storage account <vm-storage-account-name> has some active image(s) and/or disk(s), e.g. xxxxxxxxx- xxxxxxxxx-O-209490240936090599. Ensure these image(s) and/or disk(s) are removed before deleting this storage account.*</span></span>

- <span data-ttu-id="61011-123">*Účet úložiště < vm-storage účet name > má 1 kontejnery, které mít aktivní image a/nebo disk artefakty. Ujistěte se, odeberte tyto artefakty z úložiště imagí hello před odstraněním tohoto účtu úložiště*.</span><span class="sxs-lookup"><span data-stu-id="61011-123">*Storage account <vm-storage-account-name> has 1 container(s) which have an active image and/or disk artifacts. Ensure those artifacts are removed from hello image repository before deleting this storage account*.</span></span>

- <span data-ttu-id="61011-124">*Odešlete, že má účet úložiště se nezdařilo < vm-storage účet name > 1 kontejnery, které mít aktivní image a/nebo disk artefakty. Zajistěte, aby že tyto artefakty z úložiště imagí hello před odstraněním tohoto účtu úložiště. Když se pokusíte toodelete účet úložiště a stále aktivní disky s ním spojená, zobrazí se zpráva s upozorněním, nejsou aktivní disky, které je třeba odstranit toobe*.</span><span class="sxs-lookup"><span data-stu-id="61011-124">*Submit Failed Storage account <vm-storage-account-name> has 1 container(s) which have an active image and/or disk artifacts. Ensure those artifacts are removed from hello image repository before deleting this storage account. When you attempt toodelete a storage account and there are still active disks associated with it, you will see a message telling you there are active disks that need toobe deleted*.</span></span>

### <a name="scenario-2-unable-toodelete-a-container"></a><span data-ttu-id="61011-125">Scénář 2: Nelze toodelete kontejner</span><span class="sxs-lookup"><span data-stu-id="61011-125">Scenario 2: Unable toodelete a container</span></span>
<span data-ttu-id="61011-126">Když zkusíte kontejner úložiště hello toodelete, může se zobrazit hello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="61011-126">When you try toodelete hello storage container, you might see hello following error:</span></span>

<span data-ttu-id="61011-127">*Kontejner úložiště se nezdařilo toodelete <container name>. Chyba: ' hello kontejneru je aktuálně zapůjčení a žádné ID zapůjčení byl zadaný v požadavku hello*.</span><span class="sxs-lookup"><span data-stu-id="61011-127">*Failed toodelete storage container <container name>. Error: 'There is currently a lease on hello container and no lease ID was specified in hello request*.</span></span>

<span data-ttu-id="61011-128">Nebo</span><span class="sxs-lookup"><span data-stu-id="61011-128">Or</span></span>

<span data-ttu-id="61011-129">*Hello následující disky virtuálního počítače použít objekty BLOB v tomto kontejneru, nelze ho odstranit kontejner hello: VirtualMachineDiskName1, VirtualMachineDiskName2,...*</span><span class="sxs-lookup"><span data-stu-id="61011-129">*hello following virtual machine disks use blobs in this container, so hello container cannot be deleted: VirtualMachineDiskName1, VirtualMachineDiskName2, ...*</span></span>

### <a name="scenario-3-unable-toodelete-a-vhd"></a><span data-ttu-id="61011-130">Scénář 3: Nelze toodelete virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="61011-130">Scenario 3: Unable toodelete a VHD</span></span>
<span data-ttu-id="61011-131">Po odstranění virtuálního počítače a potom zkuste toodelete hello objektů blob pro hello přidružené virtuální pevné disky, se může zobrazit hello následující zprávou:</span><span class="sxs-lookup"><span data-stu-id="61011-131">After you delete a VM and then try toodelete hello blobs for hello associated VHDs, you might receive hello following message:</span></span>

<span data-ttu-id="61011-132">*Objekt blob se nezdařilo toodelete "cesta/XXXXXX-XXXXXX-os-1447379084699.vhd'. Chyba: ' u objektu blob hello je aktuálně zapůjčení a žádné ID zapůjčení byl zadaný v požadavku hello.*</span><span class="sxs-lookup"><span data-stu-id="61011-132">*Failed toodelete blob 'path/XXXXXX-XXXXXX-os-1447379084699.vhd'. Error: 'There is currently a lease on hello blob and no lease ID was specified in hello request.*</span></span>

<span data-ttu-id="61011-133">Nebo</span><span class="sxs-lookup"><span data-stu-id="61011-133">Or</span></span>

<span data-ttu-id="61011-134">*Objekt BLOB BlobName.vhd je používán jako disk virtuálního počítače, VirtualMachineDiskName', nelze ho odstranit objekt blob hello.*</span><span class="sxs-lookup"><span data-stu-id="61011-134">*Blob 'BlobName.vhd' is in use as virtual machine disk 'VirtualMachineDiskName', so hello blob cannot be deleted.*</span></span>

## <a name="solution"></a><span data-ttu-id="61011-135">Řešení</span><span class="sxs-lookup"><span data-stu-id="61011-135">Solution</span></span>
<span data-ttu-id="61011-136">tooresolve hello nejběžnějších problémů, zkuste hello následující metodu:</span><span class="sxs-lookup"><span data-stu-id="61011-136">tooresolve hello most common issues, try hello following method:</span></span>

### <a name="step-1-delete-any-disks-that-are-preventing-deletion-of-hello-storage-account-container-or-vhd"></a><span data-ttu-id="61011-137">Krok 1: Odstranění disky, které brání odstranění účtu úložiště hello, kontejneru nebo virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="61011-137">Step 1: Delete any disks that are preventing deletion of hello storage account, container, or VHD</span></span>
1. <span data-ttu-id="61011-138">Přepínač toohello [portál Azure classic](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="61011-138">Switch toohello [Azure classic portal](https://manage.windowsazure.com/).</span></span>
2. <span data-ttu-id="61011-139">Vyberte **VIRTUÁLNÍHO počítače** > **disky**.</span><span class="sxs-lookup"><span data-stu-id="61011-139">Select **VIRTUAL MACHINE** > **DISKS**.</span></span>

    ![Obrázek disky na virtuální počítače na portálu Azure classic.](./media/storage-cannot-delete-storage-account-container-vhd/VMUI.png)
3. <span data-ttu-id="61011-141">Vyhledejte hello disky, které jsou přidruženy k účtu úložiště hello, kontejneru nebo virtuálního pevného disku, které chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="61011-141">Locate hello disks that are associated with hello storage account, container, or VHD that you want toodelete.</span></span> <span data-ttu-id="61011-142">Pokud zaškrtnete hello umístění disku hello, zjistíte, že hello přidruženého účtu úložiště, kontejneru nebo virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="61011-142">When you check hello location of hello disk, you will find hello associated storage account, container, or VHD.</span></span>

    ![Obrázek, který obsahuje informace o umístění pro disky na portálu Azure classic](./media/storage-cannot-delete-storage-account-container-vhd/DiskLocation.png)
4. <span data-ttu-id="61011-144">Odstraňte hello disky pomocí jedné z následujících metod hello:</span><span class="sxs-lookup"><span data-stu-id="61011-144">Delete hello disks by using one of hello following methods:</span></span>

  - <span data-ttu-id="61011-145">Pokud je uvedený žádný virtuální počítač na hello **připojené k** pole hello disk, můžete odstranit hello disk přímo.</span><span class="sxs-lookup"><span data-stu-id="61011-145">If  there is no VM listed on hello **Attached To** field of hello disk, you can delete hello disk directly.</span></span>

  - <span data-ttu-id="61011-146">Pokud hello disk datový disk, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="61011-146">If hello disk is a data disk, follow these steps:</span></span>

    1. <span data-ttu-id="61011-147">Zkontrolujte, zda text hello název Dobrý den, který je připojený virtuální počítač, který hello disku.</span><span class="sxs-lookup"><span data-stu-id="61011-147">Check hello name of hello VM that hello disk is attached to.</span></span>
    2. <span data-ttu-id="61011-148">Přejděte příliš**virtuální počítače** > **instance**a vyhledejte hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="61011-148">Go too**Virtual Machines** > **Instances**, and then locate hello VM.</span></span>
    3. <span data-ttu-id="61011-149">Ujistěte se, že nic aktivně používá hello disku.</span><span class="sxs-lookup"><span data-stu-id="61011-149">Make sure that nothing is actively using hello disk.</span></span>
    4. <span data-ttu-id="61011-150">Vyberte **odpojit Disk** dolnímu hello hello portálu toodetach hello disku.</span><span class="sxs-lookup"><span data-stu-id="61011-150">Select **Detach Disk** at hello bottom of hello portal toodetach hello disk.</span></span>
    5. <span data-ttu-id="61011-151">Přejděte příliš**virtuální počítače** > **disky**a počkat na hello **připojené k** tooturn pole prázdné.</span><span class="sxs-lookup"><span data-stu-id="61011-151">Go too**Virtual Machines** > **Disks**, and wait for hello **Attached To** field tooturn blank.</span></span> <span data-ttu-id="61011-152">To znamená, že hello disku se úspěšně odpojil od hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="61011-152">This indicates hello disk has successfully detached from hello VM.</span></span>
    6. <span data-ttu-id="61011-153">Vyberte **odstranit** hello dolnímu okraji **virtuální počítače** > **disky** toodelete hello disku.</span><span class="sxs-lookup"><span data-stu-id="61011-153">Select **Delete** at hello bottom of **Virtual Machines** > **Disks** toodelete hello disk.</span></span>

  - <span data-ttu-id="61011-154">Pokud je disk operačního systému disku hello (hello **obsahuje operační systém** pole má hodnotu, jako je Windows) a připojené tooa virtuálních počítačů, postupujte podle těchto kroků toodelete hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="61011-154">If hello disk is an OS disk (hello **Contains OS** field has a value like Windows) and attached tooa VM, follow these steps toodelete hello VM.</span></span> <span data-ttu-id="61011-155">disk s operačním systémem Hello nejde odpojit, abychom měli toodelete hello virtuálních počítačů toorelease hello zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="61011-155">hello OS disk cannot be detached, so we have toodelete hello VM toorelease hello lease.</span></span>

    1. <span data-ttu-id="61011-156">Zkontrolujte název virtuálního počítače hello hello datový Disk je připojen k hello.</span><span class="sxs-lookup"><span data-stu-id="61011-156">Check hello name of hello Virtual Machine hello Data Disk is attached to.</span></span>  
    2. <span data-ttu-id="61011-157">Přejděte příliš**virtuální počítače** > **instance**, a pak vyberte hello virtuální počítač, který hello disku je připojen k.</span><span class="sxs-lookup"><span data-stu-id="61011-157">Go too**Virtual Machines** > **Instances**, and then select hello VM that hello disk is attached to.</span></span>
    3. <span data-ttu-id="61011-158">Ujistěte se, že nic aktivně používá hello virtuálního počítače a že jste už potřebovat hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="61011-158">Make sure that nothing is actively using hello virtual machine, and that you no longer need hello virtual machine.</span></span>
    4. <span data-ttu-id="61011-159">Vyberte hello virtuálních počítačů hello disk připojen k, pak vyberte **odstranit** > **hello odstranění připojených disků**.</span><span class="sxs-lookup"><span data-stu-id="61011-159">Select hello VM hello disk is attached to, then select **Delete** > **Delete hello attached disks**.</span></span>
    5. <span data-ttu-id="61011-160">Přejděte příliš**virtuální počítače** > **disky**a počkat na disku toodisappear hello.</span><span class="sxs-lookup"><span data-stu-id="61011-160">Go too**Virtual Machines** > **Disks**, and wait for hello disk toodisappear.</span></span>  <span data-ttu-id="61011-161">Může trvat několik minut, než tato toooccur a může být nutné stránku hello toorefresh.</span><span class="sxs-lookup"><span data-stu-id="61011-161">It may take a few minutes for this toooccur, and you may need toorefresh hello page.</span></span>
    6. <span data-ttu-id="61011-162">Pokud hello disk nezmizí, počkejte hello **připojené k** tooturn pole prázdné.</span><span class="sxs-lookup"><span data-stu-id="61011-162">If hello disk does not disappear, wait for hello **Attached To** field tooturn blank.</span></span> <span data-ttu-id="61011-163">To znamená, že hello disku má plně odpojený od hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="61011-163">This indicates hello disk has fully detached from hello VM.</span></span>  <span data-ttu-id="61011-164">Potom vyberte hello disk a vyberte **odstranit** dole hello hello stránky toodelete hello disk.</span><span class="sxs-lookup"><span data-stu-id="61011-164">Then, select hello disk, and select **Delete** at hello bottom of hello page toodelete hello disk.</span></span>


   > [!NOTE]
   > <span data-ttu-id="61011-165">Pokud disk připojené tooa virtuálního počítače, nebudete moct toodelete ho.</span><span class="sxs-lookup"><span data-stu-id="61011-165">If a disk is attached tooa VM, you will not be able toodelete it.</span></span> <span data-ttu-id="61011-166">Disky jsou asynchronně odpojit z odstraněné virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="61011-166">Disks are detached from a deleted VM asynchronously.</span></span> <span data-ttu-id="61011-167">Může trvat několik minut, po odstranění hello virtuálního počítače pro toto pole tooclear nahoru.</span><span class="sxs-lookup"><span data-stu-id="61011-167">It might take a few minutes after hello VM is deleted for this field tooclear up.</span></span>
   >
   >


### <a name="step-2-delete-any-vm-images-that-are-preventing-deletion-of-hello-storage-account-or-container"></a><span data-ttu-id="61011-168">Krok 2: Odstraňte všechny Image virtuálních počítačů, které brání odstranění účtu úložiště hello nebo kontejneru</span><span class="sxs-lookup"><span data-stu-id="61011-168">Step 2: Delete any VM Images that are preventing deletion of hello storage account or container</span></span>
1. <span data-ttu-id="61011-169">Přepínač toohello [portál Azure classic](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="61011-169">Switch toohello [Azure classic portal](https://manage.windowsazure.com/).</span></span>
2. <span data-ttu-id="61011-170">Vyberte **VIRTUÁLNÍHO počítače** > **bitové kopie**a potom odstraňte hello bitové kopie, které jsou přidruženy k účtu úložiště hello, kontejneru nebo virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="61011-170">Select **VIRTUAL MACHINE** > **IMAGES**, and then delete hello images that are associated with hello storage account, container, or VHD.</span></span>

    <span data-ttu-id="61011-171">Potom zkuste znovu účet úložiště hello toodelete, kontejneru nebo virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="61011-171">After that, try toodelete hello storage account, container, or VHD again.</span></span>

> [!WARNING]
> <span data-ttu-id="61011-172">Být jisti tooback až nic chcete toosave před odstraněním účtu hello.</span><span class="sxs-lookup"><span data-stu-id="61011-172">Be sure tooback up anything you want toosave before you delete hello account.</span></span> <span data-ttu-id="61011-173">Jakmile odstraníte virtuální pevný disk, objektů blob, tabulka, fronta nebo soubor, je trvale odstraněn.</span><span class="sxs-lookup"><span data-stu-id="61011-173">Once you delete a VHD, blob, table, queue, or file, it is permanently deleted.</span></span> <span data-ttu-id="61011-174">Ujistěte se, že hello prostředek není používán.</span><span class="sxs-lookup"><span data-stu-id="61011-174">Ensure that hello resource is not in use.</span></span>
>
>

## <a name="about-hello-stopped-deallocated-status"></a><span data-ttu-id="61011-175">O hello zastaveném (nepřiřazeném) stavu</span><span class="sxs-lookup"><span data-stu-id="61011-175">About hello Stopped (deallocated) status</span></span>
<span data-ttu-id="61011-176">Virtuální počítače, které byly vytvořené v modelu nasazení classic hello a které byly ponechány bude mít hello **zastaveném (nepřiřazeném)** stav buď hello [portál Azure](https://portal.azure.com/) nebo [portál Azure classic ](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="61011-176">VMs that were created in hello classic deployment model and that have been retained will have hello **Stopped (deallocated)** status on either hello [Azure portal](https://portal.azure.com/) or [Azure classic portal](https://manage.windowsazure.com/).</span></span>

<span data-ttu-id="61011-177">**Portál Azure classic**:</span><span class="sxs-lookup"><span data-stu-id="61011-177">**Azure classic portal**:</span></span>

![Zastavit stav (Deallocated) pro virtuální počítače na portálu Azure.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo2.png)

<span data-ttu-id="61011-179">**Portál Azure**:</span><span class="sxs-lookup"><span data-stu-id="61011-179">**Azure portal**:</span></span>

![Zastavit (deallocated) stav pro virtuální počítače na portálu Azure classic.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo1.png)

<span data-ttu-id="61011-181">Stav "Zastaveném (nepřiřazeném)" uvolní prostředky počítače hello, jako je například hello procesoru, paměti a sítě.</span><span class="sxs-lookup"><span data-stu-id="61011-181">A "Stopped (deallocated)" status releases hello computer resources, such as hello CPU, memory, and network.</span></span> <span data-ttu-id="61011-182">Hello disky, ale stále zachovány, takže můžete rychle znovu vytvořit hello virtuálních počítačů v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="61011-182">hello disks, however, are still retained so that you can quickly re-create hello VM if necessary.</span></span> <span data-ttu-id="61011-183">Tyto disky se vytvářejí na virtuálních pevných disků, které by podporovala úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="61011-183">These disks are created on top of VHDs, which are backed by Azure storage.</span></span> <span data-ttu-id="61011-184">účet úložiště Hello má tyto virtuální pevné disky a hello disky mají zapůjčení na těchto virtuálních pevných discích.</span><span class="sxs-lookup"><span data-stu-id="61011-184">hello storage account has these VHDs, and hello disks have leases on those VHDs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="61011-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="61011-185">Next steps</span></span>
* [<span data-ttu-id="61011-186">Odstranit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="61011-186">Delete a storage account</span></span>](storage-create-storage-account.md#delete-a-storage-account)
