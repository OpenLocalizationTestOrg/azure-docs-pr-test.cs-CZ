---
title: "Připojte disk k virtuálnímu počítači Azure classic | Microsoft Docs"
description: "Připojit datový disk k virtuálnímu počítači Windows vytvořené pomocí modelu nasazení classic a provést jeho inicializaci."
services: virtual-machines-windows, storage
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: be4e3e74-05bc-4527-969f-84f10a1d66a7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/21/2017
ms.author: cynthn
ms.openlocfilehash: 087d5cda354f6e1780bddd3725859444177abd16
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="attach-a-data-disk-to-a-windows-virtual-machine-created-with-the-classic-deployment-model"></a><span data-ttu-id="ea56c-103">Připojení datového disku k virtuálnímu počítači s Windows vytvořenému pomocí modelu klasického nasazení</span><span class="sxs-lookup"><span data-stu-id="ea56c-103">Attach a data disk to a Windows virtual machine created with the classic deployment model</span></span>
<!--
Refernce article:
    If you want to use the new portal, see [How to attach a data disk to a Windows VM in the Azure portal](../../virtual-machines-windows-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

<span data-ttu-id="ea56c-104">Tento článek ukazuje, jak připojit nové a stávající disky vytvořené pomocí modelu nasazení Classic do virtuálního počítače s Windows pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ea56c-104">This article shows you how to attach new and existing disks created with the Classic deployment model to a Windows virtual machine using the Azure portal.</span></span>

<span data-ttu-id="ea56c-105">Můžete také [připojit datový disk do virtuálního počítače s Linuxem na portálu Azure](../../linux/attach-disk-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ea56c-105">You can also [attach a data disk to a Linux VM in the Azure portal](../../linux/attach-disk-portal.md).</span></span>

<span data-ttu-id="ea56c-106">Než připojíte disk, projděte si tyto tipy:</span><span class="sxs-lookup"><span data-stu-id="ea56c-106">Before you attach a disk, review these tips:</span></span>

* <span data-ttu-id="ea56c-107">Velikost virtuálního počítače určuje, kolik datových disků můžete připojit.</span><span class="sxs-lookup"><span data-stu-id="ea56c-107">The size of the virtual machine controls how many data disks you can attach.</span></span> <span data-ttu-id="ea56c-108">Podrobnosti najdete v tématu [velikosti virtuálních počítačů](../../virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ea56c-108">For details, see [Sizes for virtual machines](../../virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

* <span data-ttu-id="ea56c-109">Chcete-li používat úložiště úrovně Premium, je třeba DS-series nebo GS-series virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ea56c-109">To use Premium storage, you need a DS-series or GS-series virtual machine.</span></span> <span data-ttu-id="ea56c-110">Můžete použít disky z účty úložiště Premium a Standard s těchto virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ea56c-110">You can use disks from both Premium and Standard storage accounts with these virtual machines.</span></span> <span data-ttu-id="ea56c-111">Storage úrovně Premium je k dispozici v určité oblasti.</span><span class="sxs-lookup"><span data-stu-id="ea56c-111">Premium storage is available in certain regions.</span></span> <span data-ttu-id="ea56c-112">Podrobnosti najdete v tématu [úložiště Premium: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ea56c-112">For details, see [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](../../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

* <span data-ttu-id="ea56c-113">Pro nový disk nemusíte nejdřív ji vytvořit, protože Azure ji vytvoří, když jeho připojení.</span><span class="sxs-lookup"><span data-stu-id="ea56c-113">For a new disk, you don't need to create it first because Azure creates it when you attach it.</span></span>

<span data-ttu-id="ea56c-114">Můžete také [připojit datový disk pomocí prostředí Powershell](../../virtual-machines-windows-attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="ea56c-114">You can also [attach a data disk using Powershell](../../virtual-machines-windows-attach-disk-ps.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ea56c-115">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="ea56c-115">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span>

## <a name="find-the-virtual-machine"></a><span data-ttu-id="ea56c-116">Najít virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="ea56c-116">Find the virtual machine</span></span>
1. <span data-ttu-id="ea56c-117">Přihlaste se k webu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="ea56c-117">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="ea56c-118">Vyberte virtuální počítač z prostředku uvedené na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="ea56c-118">Select the virtual machine from the resource listed on the dashboard.</span></span>
3. <span data-ttu-id="ea56c-119">V levém podokně v části **nastavení**, klikněte na tlačítko **disky**.</span><span class="sxs-lookup"><span data-stu-id="ea56c-119">In the left pane under **Settings**, click **Disks**.</span></span>

    ![Otevřete nastavení disku](./media/attach-disk/virtualmachinedisks.png)

<span data-ttu-id="ea56c-121">Pokračujte podle pokynů pro připojení buď [nový disk](#option-1-attach-a-new-disk) nebo [stávající disk](#option-2-attach-an-existing-disk).</span><span class="sxs-lookup"><span data-stu-id="ea56c-121">Continue by following instructions for attaching either a [new disk](#option-1-attach-a-new-disk) or an [existing disk](#option-2-attach-an-existing-disk).</span></span>

## <a name="option-1-attach-and-initialize-a-new-disk"></a><span data-ttu-id="ea56c-122">Možnost 1: Připojte a inicializace nový disk</span><span class="sxs-lookup"><span data-stu-id="ea56c-122">Option 1: Attach and initialize a new disk</span></span>

1. <span data-ttu-id="ea56c-123">Na **disky** okně klikněte na tlačítko **připojit nový**.</span><span class="sxs-lookup"><span data-stu-id="ea56c-123">On the **Disks** blade, click **Attach new**.</span></span>
2. <span data-ttu-id="ea56c-124">Zkontrolujte výchozí nastavení, aktualizovat podle potřeby a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ea56c-124">Review the default settings, update as necessary, and then click **OK**.</span></span>

   ![Zkontrolujte nastavení disku](./media/attach-disk/attach-new.png)

3. <span data-ttu-id="ea56c-126">Jakmile Azure vytvoří disk a připojí jej k virtuálnímu počítači, na nový disk, je uvedena ve nastavení disku virtuálního počítače v části **datových disků**.</span><span class="sxs-lookup"><span data-stu-id="ea56c-126">After Azure creates the disk and attaches it to the virtual machine, the new disk is listed in the virtual machine's disk settings under **Data Disks**.</span></span>

### <a name="initialize-a-new-data-disk"></a><span data-ttu-id="ea56c-127">Inicializace nový datový disk</span><span class="sxs-lookup"><span data-stu-id="ea56c-127">Initialize a new data disk</span></span>

1. <span data-ttu-id="ea56c-128">Připojte k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="ea56c-128">Connect to the virtual machine.</span></span> <span data-ttu-id="ea56c-129">Pokyny najdete v tématu [jak se připojit a přihlásit se na virtuálním počítači Azure s Windows](../../virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ea56c-129">For instructions, see [How to connect and log on to an Azure virtual machine running Windows](../../virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
2. <span data-ttu-id="ea56c-130">Po přihlášení k virtuálnímu počítači otevřete **správce serveru**.</span><span class="sxs-lookup"><span data-stu-id="ea56c-130">After you log on to the virtual machine, open **Server Manager**.</span></span> <span data-ttu-id="ea56c-131">V levém podokně vyberte **Souborová služba a služba úložiště**.</span><span class="sxs-lookup"><span data-stu-id="ea56c-131">In the left pane, select **File and Storage Services**.</span></span>

    ![Otevřete správce serveru](../media/attach-disk-portal/fileandstorageservices.png)

3. <span data-ttu-id="ea56c-133">Vyberte **disky**.</span><span class="sxs-lookup"><span data-stu-id="ea56c-133">Select **Disks**.</span></span>
4. <span data-ttu-id="ea56c-134">**Disky** části jsou uvedené disky.</span><span class="sxs-lookup"><span data-stu-id="ea56c-134">The **Disks** section lists the disks.</span></span> <span data-ttu-id="ea56c-135">Virtuální počítač má nejčastěji disk 0, disk 1 a 2 disku.</span><span class="sxs-lookup"><span data-stu-id="ea56c-135">Most often, a virtual machine has disk 0, disk 1, and disk 2.</span></span> <span data-ttu-id="ea56c-136">Disk 0 je disk operačního systému, disk 1 je dočasný disk a disk 2 je datový disk nově připojen k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="ea56c-136">Disk 0 is the operating system disk, disk 1 is the temporary disk, and disk 2 is the data disk newly attached to the virtual machine.</span></span> <span data-ttu-id="ea56c-137">Datový disk zobrazí oddíl jako **neznámé**.</span><span class="sxs-lookup"><span data-stu-id="ea56c-137">The data disk lists the Partition as **Unknown**.</span></span>

 <span data-ttu-id="ea56c-138">Klikněte pravým tlačítkem na disk a vyberte **inicializovat**.</span><span class="sxs-lookup"><span data-stu-id="ea56c-138">Right-click the disk and select **Initialize**.</span></span>

5. <span data-ttu-id="ea56c-139">Zobrazení upozornění, že všechna data vymažou se při inicializaci disku.</span><span class="sxs-lookup"><span data-stu-id="ea56c-139">You're notified that all data will be erased when the disk is initialized.</span></span> <span data-ttu-id="ea56c-140">Klikněte na tlačítko **Ano** upozornění na vědomí a inicializujte disk.</span><span class="sxs-lookup"><span data-stu-id="ea56c-140">Click **Yes** to acknowledge the warning and initialize the disk.</span></span> <span data-ttu-id="ea56c-141">Po dokončení oddíl bude uvedený jako **GPT**.</span><span class="sxs-lookup"><span data-stu-id="ea56c-141">Once complete, the partition will be listed as **GPT**.</span></span> <span data-ttu-id="ea56c-142">Znovu klikněte pravým tlačítkem na disk a vyberte **nový svazek**.</span><span class="sxs-lookup"><span data-stu-id="ea56c-142">Right-click the disk again and select **New Volume**.</span></span>

6. <span data-ttu-id="ea56c-143">Dokončete průvodce pomocí výchozích hodnot.</span><span class="sxs-lookup"><span data-stu-id="ea56c-143">Complete the wizard using the default values.</span></span> <span data-ttu-id="ea56c-144">Po dokončení průvodce **svazky** části je uveden seznam nového svazku.</span><span class="sxs-lookup"><span data-stu-id="ea56c-144">When the wizard is done, the **Volumes** section lists the new volume.</span></span> <span data-ttu-id="ea56c-145">Disk je teď online a připravená k ukládání dat.</span><span class="sxs-lookup"><span data-stu-id="ea56c-145">The disk is now online and ready to store data.</span></span>

    ![Svazek úspěšně inicializován.](./media/attach-disk/newdiskafterinitialization.png)

## <a name="option-2-attach-an-existing-disk"></a><span data-ttu-id="ea56c-147">Možnost 2: Připojit stávající disk</span><span class="sxs-lookup"><span data-stu-id="ea56c-147">Option 2: Attach an existing disk</span></span>
1. <span data-ttu-id="ea56c-148">Na **disky** okně klikněte na tlačítko **připojit existující**.</span><span class="sxs-lookup"><span data-stu-id="ea56c-148">On the **Disks** blade, click **Attach existing**.</span></span>
2. <span data-ttu-id="ea56c-149">V části **připojit stávající disk**, klikněte na tlačítko **umístění**.</span><span class="sxs-lookup"><span data-stu-id="ea56c-149">Under **Attach existing disk**, click **Location**.</span></span>

   ![Připojit stávající disk](./media/attach-disk/attachexistingdisksettings.png)
3. <span data-ttu-id="ea56c-151">V části **účty úložiště**, vyberte účet a kontejner, který obsahuje soubor VHD.</span><span class="sxs-lookup"><span data-stu-id="ea56c-151">Under **Storage accounts**, select the account and container that holds the .vhd file.</span></span>

   ![Vyhledejte umístění virtuálního pevného disku](./media/attach-disk/existdiskstorageaccountandcontainer.png)

4. <span data-ttu-id="ea56c-153">Vyberte soubor VHD.</span><span class="sxs-lookup"><span data-stu-id="ea56c-153">Select the .vhd file.</span></span>
5. <span data-ttu-id="ea56c-154">V části **připojit stávající disk**, právě vybraný soubor je uveden v části **souboru virtuálního pevného disku**.</span><span class="sxs-lookup"><span data-stu-id="ea56c-154">Under **Attach existing disk**, the file you just selected is listed under **VHD File**.</span></span> <span data-ttu-id="ea56c-155">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ea56c-155">Click **OK**.</span></span>
6. <span data-ttu-id="ea56c-156">Po Azure připojí disk k virtuálnímu počítači, je uvedena v nastavení disku virtuálního počítače v části **datových disků**.</span><span class="sxs-lookup"><span data-stu-id="ea56c-156">After Azure attaches the disk to the virtual machine, it's listed in the virtual machine's disk settings under **Data Disks**.</span></span>

## <a name="use-trim-with-standard-storage"></a><span data-ttu-id="ea56c-157">Použít standardní úložiště uvolnění dočasné paměti</span><span class="sxs-lookup"><span data-stu-id="ea56c-157">Use TRIM with standard storage</span></span>

<span data-ttu-id="ea56c-158">Pokud chcete použít standardní úložiště (HDD), měli byste povolit uvolnění dočasné paměti.</span><span class="sxs-lookup"><span data-stu-id="ea56c-158">If you use standard storage (HDD), you should enable TRIM.</span></span> <span data-ttu-id="ea56c-159">TRIM zahodí nepoužívané bloky na disku, můžete se účtují pouze pro úložiště, které skutečně používáte.</span><span class="sxs-lookup"><span data-stu-id="ea56c-159">TRIM discards unused blocks on the disk so you are only billed for storage that you are actually using.</span></span> <span data-ttu-id="ea56c-160">Pomocí uvolnění dočasné paměti může významně snížit náklady, včetně nepoužívané bloky, které jsou výsledkem odstranění velkých souborů.</span><span class="sxs-lookup"><span data-stu-id="ea56c-160">Using TRIM can save costs, including unused blocks that result from deleting large files.</span></span>

<span data-ttu-id="ea56c-161">Můžete spustit tento příkaz a zkontrolujte nastavení uvolnění dočasné paměti.</span><span class="sxs-lookup"><span data-stu-id="ea56c-161">You can run this command to check the TRIM setting.</span></span> <span data-ttu-id="ea56c-162">Otevřete příkazový řádek na virtuální počítač s Windows a zadejte:</span><span class="sxs-lookup"><span data-stu-id="ea56c-162">Open a command prompt on your Windows VM and type:</span></span>

```
fsutil behavior query DisableDeleteNotify
```

<span data-ttu-id="ea56c-163">Pokud příkaz vrátí hodnotu 0, TRIM správně povolené.</span><span class="sxs-lookup"><span data-stu-id="ea56c-163">If the command returns 0, TRIM is enabled correctly.</span></span> <span data-ttu-id="ea56c-164">Pokud vrátí hodnotu 1, spusťte následující příkaz k povolení uvolnění dočasné paměti:</span><span class="sxs-lookup"><span data-stu-id="ea56c-164">If it returns 1, run the following command to enable TRIM:</span></span>
```
fsutil behavior set DisableDeleteNotify 0
```

## <a name="next-steps"></a><span data-ttu-id="ea56c-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ea56c-165">Next steps</span></span>
<span data-ttu-id="ea56c-166">Pokud aplikace potřebuje používat D: disk k uložení dat, můžete [změnit písmeno jednotky dočasné disk systému Windows](../../virtual-machines-windows-change-drive-letter.md).</span><span class="sxs-lookup"><span data-stu-id="ea56c-166">If your application needs to use the D: drive to store data, you can [change the drive letter of the Windows temporary disk](../../virtual-machines-windows-change-drive-letter.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ea56c-167">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ea56c-167">Additional resources</span></span>
[<span data-ttu-id="ea56c-168">O disky a virtuální pevné disky pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="ea56c-168">About disks and VHDs for virtual machines</span></span>](../../virtual-machines-linux-about-disks-vhds.md)
