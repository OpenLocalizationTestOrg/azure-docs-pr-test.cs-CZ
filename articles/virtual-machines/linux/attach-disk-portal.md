---
title: "Připojit datový disk do virtuálního počítače s Linuxem | Microsoft Docs"
description: "Tom, jak připojit nové nebo stávající datový disk do virtuálního počítače s Linuxem v portálu Azure pomocí modelu nasazení Resource Manager."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 5e1c6212-976c-4962-a297-177942f90907
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: cynthn
ms.openlocfilehash: 1599ee241c3d9fb3623ebd89ae30f2795cae1930
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-attach-a-data-disk-to-a-linux-vm-in-the-azure-portal"></a><span data-ttu-id="1f1e6-103">Tom, jak připojit datový disk do virtuálního počítače s Linuxem na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="1f1e6-103">How to attach a data disk to a Linux VM in the Azure portal</span></span>
<span data-ttu-id="1f1e6-104">Tento článek ukazuje, jak připojit nové i stávající disky pro virtuální počítač s Linuxem pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-104">This article shows you how to attach both new and existing disks to a Linux virtual machine through the Azure portal.</span></span> <span data-ttu-id="1f1e6-105">Můžete také [připojit datový disk k virtuálnímu počítači Windows na portálu Azure](../windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1f1e6-105">You can also [attach a data disk to a Windows VM in the Azure portal](../windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="1f1e6-106">Můžete použít buď disky Azure spravované nebo nespravované disky.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-106">You can choose to use either Azure Managed Disks or unmanaged disks.</span></span> <span data-ttu-id="1f1e6-107">Spravované disky jsou zpracovávány platformy Azure a nevyžadují, aby všechny přípravné nebo umístění pro uložení.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-107">Managed disks are handled by the Azure platform and do not require any preparation or location to store them.</span></span> <span data-ttu-id="1f1e6-108">Nespravované disky se vyžaduje účet úložiště a mají některé [kvóty a omezení, které se vztahují](../../azure-subscription-service-limits.md#storage-limits).</span><span class="sxs-lookup"><span data-stu-id="1f1e6-108">Unmanaged disks require a storage account and have some [quotas and limits that apply](../../azure-subscription-service-limits.md#storage-limits).</span></span> <span data-ttu-id="1f1e6-109">Další informace o službě Azure Managed Disks najdete v tématu [Přehled služby Azure Managed Disks](../windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1f1e6-109">For more information about Azure Managed Disks, see [Azure Managed Disks overview](../windows/managed-disks-overview.md).</span></span>

<span data-ttu-id="1f1e6-110">Než připojíte k virtuálnímu počítači disky, projděte si tyto tipy:</span><span class="sxs-lookup"><span data-stu-id="1f1e6-110">Before you attach disks to your VM, review these tips:</span></span>

* <span data-ttu-id="1f1e6-111">Velikost virtuálního počítače určuje, kolik datových disků můžete připojit.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-111">The size of the virtual machine controls how many data disks you can attach.</span></span> <span data-ttu-id="1f1e6-112">Podrobnosti najdete v tématu [velikosti virtuálních počítačů](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1f1e6-112">For details, see [Sizes for virtual machines](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* <span data-ttu-id="1f1e6-113">Chcete-li používat úložiště úrovně Premium, je třeba DS-series nebo GS-series virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-113">To use Premium storage, you need a DS-series or GS-series virtual machine.</span></span> <span data-ttu-id="1f1e6-114">Pomocí těchto virtuálních počítačů můžete použít Premium i standardní disky.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-114">You can use both Premium and Standard disks with these virtual machines.</span></span> <span data-ttu-id="1f1e6-115">Storage úrovně Premium je k dispozici v určité oblasti.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-115">Premium storage is available in certain regions.</span></span> <span data-ttu-id="1f1e6-116">Podrobnosti najdete v tématu [úložiště Premium: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1f1e6-116">For details, see [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* <span data-ttu-id="1f1e6-117">Disky připojené k virtuální počítače jsou ve skutečnosti soubory VHD uložených v Azure.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-117">Disks attached to virtual machines are actually .vhd files stored in Azure.</span></span> <span data-ttu-id="1f1e6-118">Podrobnosti najdete v tématu [o disky a virtuální pevné disky pro virtuální počítače](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1f1e6-118">For details, see [About disks and VHDs for virtual machines](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


## <a name="find-the-virtual-machine"></a><span data-ttu-id="1f1e6-119">Najít virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="1f1e6-119">Find the virtual machine</span></span>
1. <span data-ttu-id="1f1e6-120">Přihlaste se k webu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="1f1e6-120">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="1f1e6-121">V nabídce centra klikněte na **Virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-121">On the Hub menu, click **Virtual Machines**.</span></span>
3. <span data-ttu-id="1f1e6-122">Ze seznamu vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-122">Select the virtual machine from the list.</span></span>
4. <span data-ttu-id="1f1e6-123">K virtuální počítače, v okně **Essentials**, klikněte na tlačítko **disky**.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-123">To the Virtual machines blade, in **Essentials**, click **Disks**.</span></span>
   
    ![Otevřete nastavení disku](./media/attach-disk-portal/find-disk-settings.png)

<span data-ttu-id="1f1e6-125">Pokračujte podle pokynů pro připojení buď [spravovaných disků na](#use-azure-managed-disks) nebo [nespravované disku](#use-unmanaged-disks).</span><span class="sxs-lookup"><span data-stu-id="1f1e6-125">Continue by following instructions for attaching either a [managed disk](#use-azure-managed-disks) or [unmanaged disk](#use-unmanaged-disks).</span></span>

## <a name="use-azure-managed-disks"></a><span data-ttu-id="1f1e6-126">Použití spravovaných disky systému Azure</span><span class="sxs-lookup"><span data-stu-id="1f1e6-126">Use Azure Managed Disks</span></span>

### <a name="attach-a-new-disk"></a><span data-ttu-id="1f1e6-127">Připojit nový disk</span><span class="sxs-lookup"><span data-stu-id="1f1e6-127">Attach a new disk</span></span>

1. <span data-ttu-id="1f1e6-128">Na **disky** okně klikněte na tlačítko **+ přidat datový disk**.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-128">On the **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="1f1e6-129">Klikněte na rozevírací nabídku pro **název** a vyberte **vytvořit disk**:</span><span class="sxs-lookup"><span data-stu-id="1f1e6-129">Click the drop-down menu for **Name** and select **Create disk**:</span></span>

    ![Vytvoření Azure spravovaných disků](./media/attach-disk-portal/create-new-md.png)

3. <span data-ttu-id="1f1e6-131">Zadejte název pro spravované disk.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-131">Enter a name for your managed disk.</span></span> <span data-ttu-id="1f1e6-132">Zkontrolujte výchozí nastavení, aktualizovat podle potřeby a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-132">Review the default settings, update as necessary, and then click **Create**.</span></span>
   
   ![Zkontrolujte nastavení disku](./media/attach-disk-portal/create-new-md-settings.png)

4. <span data-ttu-id="1f1e6-134">Klikněte na tlačítko **Uložit** k vytvoření spravovaného disku a aktualizaci konfigurace virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="1f1e6-134">Click **Save** to create the managed disk and update the VM configuration:</span></span>

   ![Uložit nový Disk spravované Azure](./media/attach-disk-portal/confirm-create-new-md.png)

5. <span data-ttu-id="1f1e6-136">Jakmile Azure vytvoří disk a připojí jej k virtuálnímu počítači, na nový disk, je uvedena ve nastavení disku virtuálního počítače v části **datových disků**.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-136">After Azure creates the disk and attaches it to the virtual machine, the new disk is listed in the virtual machine's disk settings under **Data Disks**.</span></span> <span data-ttu-id="1f1e6-137">Nejvyšší úrovně prostředků jsou spravovaných disků disk se zobrazí v kořenovém adresáři skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="1f1e6-137">As managed disks are a top-level resource, the disk appears at the root of the resource group:</span></span>

   ![Disk Azure spravované ve skupině prostředků](./media/attach-disk-portal/view-md-resource-group.png)

### <a name="attach-an-existing-disk"></a><span data-ttu-id="1f1e6-139">Připojení stávajícího disku</span><span class="sxs-lookup"><span data-stu-id="1f1e6-139">Attach an existing disk</span></span>
1. <span data-ttu-id="1f1e6-140">Na **disky** okně klikněte na tlačítko **+ přidat datový disk**.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-140">On the **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="1f1e6-141">Klikněte na rozevírací nabídku pro **název** zobrazíte seznam existující spravovaných disků, které jsou přístupné pro vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-141">Click the drop-down menu for **Name** to view a list of existing managed disks accessible to your Azure subscription.</span></span> <span data-ttu-id="1f1e6-142">Vyberte spravované disk připojit:</span><span class="sxs-lookup"><span data-stu-id="1f1e6-142">Select the managed disk to attach:</span></span>

   ![Připojit stávající Disk spravované Azure](./media/attach-disk-portal/select-existing-md.png)

3. <span data-ttu-id="1f1e6-144">Klikněte na tlačítko **Uložit** připojit stávající disk spravované a aktualizaci konfigurace virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="1f1e6-144">Click **Save** to attach the existing managed disk and update the VM configuration:</span></span>
   
   ![Uložte aktualizace disku spravované Azure](./media/attach-disk-portal/confirm-attach-existing-md.png)

4. <span data-ttu-id="1f1e6-146">Po Azure připojí disk k virtuálnímu počítači, je uvedena v nastavení disku virtuálního počítače v části **datových disků**.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-146">After Azure attaches the disk to the virtual machine, it's listed in the virtual machine's disk settings under **Data Disks**.</span></span>

## <a name="use-unmanaged-disks"></a><span data-ttu-id="1f1e6-147">Použití nespravovaného disky</span><span class="sxs-lookup"><span data-stu-id="1f1e6-147">Use unmanaged disks</span></span>

### <a name="attach-a-new-disk"></a><span data-ttu-id="1f1e6-148">Připojit nový disk</span><span class="sxs-lookup"><span data-stu-id="1f1e6-148">Attach a new disk</span></span>

1. <span data-ttu-id="1f1e6-149">Na **disky** okně klikněte na tlačítko **+ přidat datový disk**.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-149">On the **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="1f1e6-150">Zkontrolujte výchozí nastavení, aktualizovat podle potřeby a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-150">Review the default settings, update as necessary, and then click **OK**.</span></span>
   
   ![Zkontrolujte nastavení disku](./media/attach-disk-portal/attach-new.png)
3. <span data-ttu-id="1f1e6-152">Jakmile Azure vytvoří disk a připojí jej k virtuálnímu počítači, na nový disk, je uvedena ve nastavení disku virtuálního počítače v části **datových disků**.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-152">After Azure creates the disk and attaches it to the virtual machine, the new disk is listed in the virtual machine's disk settings under **Data Disks**.</span></span>

### <a name="attach-an-existing-disk"></a><span data-ttu-id="1f1e6-153">Připojení stávajícího disku</span><span class="sxs-lookup"><span data-stu-id="1f1e6-153">Attach an existing disk</span></span>
1. <span data-ttu-id="1f1e6-154">Na **disky** okně klikněte na tlačítko **+ přidat datový disk**.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-154">On the **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="1f1e6-155">V části **připojit stávající disk**, klikněte na tlačítko **souboru virtuálního pevného disku**.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-155">Under **Attach existing disk**, click **VHD File**.</span></span>
   
   ![Připojit stávající disk](./media/attach-disk-portal/attach-existing.png)
3. <span data-ttu-id="1f1e6-157">V části **účty úložiště**, vyberte účet a kontejner, který obsahuje soubor VHD.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-157">Under **Storage accounts**, select the account and container that holds the .vhd file.</span></span>
   
   ![Vyhledejte umístění virtuálního pevného disku](./media/attach-disk-portal/find-storage-container.png)
4. <span data-ttu-id="1f1e6-159">Vyberte soubor VHD.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-159">Select the .vhd file.</span></span>
5. <span data-ttu-id="1f1e6-160">V části **připojit stávající disk**, právě vybraný soubor je uveden v části **souboru virtuálního pevného disku**.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-160">Under **Attach existing disk**, the file you just selected is listed under **VHD File**.</span></span> <span data-ttu-id="1f1e6-161">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-161">Click **OK**.</span></span>
6. <span data-ttu-id="1f1e6-162">Po Azure připojí disk k virtuálnímu počítači, je uvedena v nastavení disku virtuálního počítače v části **datových disků**.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-162">After Azure attaches the disk to the virtual machine, it's listed in the virtual machine's disk settings under **Data Disks**.</span></span>


## <a name="next-steps"></a><span data-ttu-id="1f1e6-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1f1e6-163">Next steps</span></span>
<span data-ttu-id="1f1e6-164">Po přidání disku budete muset připravit k použití.</span><span class="sxs-lookup"><span data-stu-id="1f1e6-164">After the disk is added, you need to prepare it for use.</span></span> <span data-ttu-id="1f1e6-165">Další informace najdete v tématu [postupy: Inicializace nový datový disk v systému Linux](add-disk.md).</span><span class="sxs-lookup"><span data-stu-id="1f1e6-165">For more information, see [How to: Initialize a new data disk in Linux](add-disk.md).</span></span>
