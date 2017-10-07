---
title: "aaaAttach tooa datového disku virtuálního počítače s Linuxem | Microsoft Docs"
description: "Jak tooattach nových nebo existujících dat disku tooa virtuálního počítače s Linuxem v Azure pomocí portálu hello hello modelu nasazení Resource Manager."
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
ms.openlocfilehash: 069c3c6f5e71a8c495342e6d0c6f3d92c4ed5053
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooattach-a-data-disk-tooa-linux-vm-in-hello-azure-portal"></a><span data-ttu-id="16777-103">Jak tooattach datový disk tooa virtuálního počítače s Linuxem v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="16777-103">How tooattach a data disk tooa Linux VM in hello Azure portal</span></span>
<span data-ttu-id="16777-104">Tento článek ukazuje, jak tooattach nové i stávající disky tooa Linux virtuálnímu počítači prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="16777-104">This article shows you how tooattach both new and existing disks tooa Linux virtual machine through hello Azure portal.</span></span> <span data-ttu-id="16777-105">Můžete také [připojit tooa disku data virtuálního počítače s Windows v portálu Azure hello](../windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="16777-105">You can also [attach a data disk tooa Windows VM in hello Azure portal](../windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="16777-106">Toouse můžete zvolit buď disky Azure spravované nebo nespravované disky.</span><span class="sxs-lookup"><span data-stu-id="16777-106">You can choose toouse either Azure Managed Disks or unmanaged disks.</span></span> <span data-ttu-id="16777-107">Spravované disky jsou zpracovávány hello platformy Azure a nevyžadují, aby všechny přípravné nebo umístění toostore je.</span><span class="sxs-lookup"><span data-stu-id="16777-107">Managed disks are handled by hello Azure platform and do not require any preparation or location toostore them.</span></span> <span data-ttu-id="16777-108">Nespravované disky se vyžaduje účet úložiště a mají některé [kvóty a omezení, které se vztahují](../../azure-subscription-service-limits.md#storage-limits).</span><span class="sxs-lookup"><span data-stu-id="16777-108">Unmanaged disks require a storage account and have some [quotas and limits that apply](../../azure-subscription-service-limits.md#storage-limits).</span></span> <span data-ttu-id="16777-109">Další informace o službě Azure Managed Disks najdete v tématu [Přehled služby Azure Managed Disks](../windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="16777-109">For more information about Azure Managed Disks, see [Azure Managed Disks overview](../windows/managed-disks-overview.md).</span></span>

<span data-ttu-id="16777-110">Než připojíte tooyour disky virtuálního počítače, projděte si tyto tipy:</span><span class="sxs-lookup"><span data-stu-id="16777-110">Before you attach disks tooyour VM, review these tips:</span></span>

* <span data-ttu-id="16777-111">velikost Hello hello virtuálního počítače určuje, kolik datových disků můžete připojit.</span><span class="sxs-lookup"><span data-stu-id="16777-111">hello size of hello virtual machine controls how many data disks you can attach.</span></span> <span data-ttu-id="16777-112">Podrobnosti najdete v tématu [velikosti virtuálních počítačů](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="16777-112">For details, see [Sizes for virtual machines](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* <span data-ttu-id="16777-113">toouse storage úrovně Premium, budete potřebovat DS-series nebo GS-series virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="16777-113">toouse Premium storage, you need a DS-series or GS-series virtual machine.</span></span> <span data-ttu-id="16777-114">Pomocí těchto virtuálních počítačů můžete použít Premium i standardní disky.</span><span class="sxs-lookup"><span data-stu-id="16777-114">You can use both Premium and Standard disks with these virtual machines.</span></span> <span data-ttu-id="16777-115">Storage úrovně Premium je k dispozici v určité oblasti.</span><span class="sxs-lookup"><span data-stu-id="16777-115">Premium storage is available in certain regions.</span></span> <span data-ttu-id="16777-116">Podrobnosti najdete v tématu [úložiště Premium: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="16777-116">For details, see [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* <span data-ttu-id="16777-117">Disky připojené toovirtual počítače jsou ve skutečnosti soubory VHD uložených v Azure.</span><span class="sxs-lookup"><span data-stu-id="16777-117">Disks attached toovirtual machines are actually .vhd files stored in Azure.</span></span> <span data-ttu-id="16777-118">Podrobnosti najdete v tématu [o disky a virtuální pevné disky pro virtuální počítače](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="16777-118">For details, see [About disks and VHDs for virtual machines](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


## <a name="find-hello-virtual-machine"></a><span data-ttu-id="16777-119">Najít hello virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="16777-119">Find hello virtual machine</span></span>
1. <span data-ttu-id="16777-120">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="16777-120">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="16777-121">V nabídce centra hello, klikněte na tlačítko **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="16777-121">On hello Hub menu, click **Virtual Machines**.</span></span>
3. <span data-ttu-id="16777-122">Vyberte virtuální počítač hello hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="16777-122">Select hello virtual machine from hello list.</span></span>
4. <span data-ttu-id="16777-123">toohello virtuální počítače, v okně **Essentials**, klikněte na tlačítko **disky**.</span><span class="sxs-lookup"><span data-stu-id="16777-123">toohello Virtual machines blade, in **Essentials**, click **Disks**.</span></span>
   
    ![Otevřete nastavení disku](./media/attach-disk-portal/find-disk-settings.png)

<span data-ttu-id="16777-125">Pokračujte podle pokynů pro připojení buď [spravovaných disků na](#use-azure-managed-disks) nebo [nespravované disku](#use-unmanaged-disks).</span><span class="sxs-lookup"><span data-stu-id="16777-125">Continue by following instructions for attaching either a [managed disk](#use-azure-managed-disks) or [unmanaged disk](#use-unmanaged-disks).</span></span>

## <a name="use-azure-managed-disks"></a><span data-ttu-id="16777-126">Použití spravovaných disky systému Azure</span><span class="sxs-lookup"><span data-stu-id="16777-126">Use Azure Managed Disks</span></span>

### <a name="attach-a-new-disk"></a><span data-ttu-id="16777-127">Připojit nový disk</span><span class="sxs-lookup"><span data-stu-id="16777-127">Attach a new disk</span></span>

1. <span data-ttu-id="16777-128">Na hello **disky** okně klikněte na tlačítko **+ přidat datový disk**.</span><span class="sxs-lookup"><span data-stu-id="16777-128">On hello **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="16777-129">Klikněte na rozevírací nabídku hello **název** a vyberte **vytvořit disk**:</span><span class="sxs-lookup"><span data-stu-id="16777-129">Click hello drop-down menu for **Name** and select **Create disk**:</span></span>

    ![Vytvoření Azure spravovaných disků](./media/attach-disk-portal/create-new-md.png)

3. <span data-ttu-id="16777-131">Zadejte název pro spravované disk.</span><span class="sxs-lookup"><span data-stu-id="16777-131">Enter a name for your managed disk.</span></span> <span data-ttu-id="16777-132">Zkontrolujte hello výchozí nastavení, aktualizovat podle potřeby a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="16777-132">Review hello default settings, update as necessary, and then click **Create**.</span></span>
   
   ![Zkontrolujte nastavení disku](./media/attach-disk-portal/create-new-md-settings.png)

4. <span data-ttu-id="16777-134">Klikněte na tlačítko **Uložit** toocreate hello spravované disku a aktualizaci konfigurace virtuálního počítače hello:</span><span class="sxs-lookup"><span data-stu-id="16777-134">Click **Save** toocreate hello managed disk and update hello VM configuration:</span></span>

   ![Uložit nový Disk spravované Azure](./media/attach-disk-portal/confirm-create-new-md.png)

5. <span data-ttu-id="16777-136">Jakmile Azure vytvoří hello disk a připojí jej toohello virtuálního počítače, hello nového disku je uvedena v nastavení disku hello virtuálního počítače v části **datových disků**.</span><span class="sxs-lookup"><span data-stu-id="16777-136">After Azure creates hello disk and attaches it toohello virtual machine, hello new disk is listed in hello virtual machine's disk settings under **Data Disks**.</span></span> <span data-ttu-id="16777-137">Prostředek nejvyšší úrovně jsou spravované disky hello disk se zobrazí v kořenu hello hello skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="16777-137">As managed disks are a top-level resource, hello disk appears at hello root of hello resource group:</span></span>

   ![Disk Azure spravované ve skupině prostředků](./media/attach-disk-portal/view-md-resource-group.png)

### <a name="attach-an-existing-disk"></a><span data-ttu-id="16777-139">Připojení stávajícího disku</span><span class="sxs-lookup"><span data-stu-id="16777-139">Attach an existing disk</span></span>
1. <span data-ttu-id="16777-140">Na hello **disky** okně klikněte na tlačítko **+ přidat datový disk**.</span><span class="sxs-lookup"><span data-stu-id="16777-140">On hello **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="16777-141">Klikněte na rozevírací nabídku hello **název** tooview seznam existujících spravované tooyour dostupné disky předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="16777-141">Click hello drop-down menu for **Name** tooview a list of existing managed disks accessible tooyour Azure subscription.</span></span> <span data-ttu-id="16777-142">Vyberte hello spravované tooattach disku:</span><span class="sxs-lookup"><span data-stu-id="16777-142">Select hello managed disk tooattach:</span></span>

   ![Připojit stávající Disk spravované Azure](./media/attach-disk-portal/select-existing-md.png)

3. <span data-ttu-id="16777-144">Klikněte na tlačítko **Uložit** tooattach hello existující spravované disku a aktualizaci konfigurace virtuálního počítače hello:</span><span class="sxs-lookup"><span data-stu-id="16777-144">Click **Save** tooattach hello existing managed disk and update hello VM configuration:</span></span>
   
   ![Uložte aktualizace disku spravované Azure](./media/attach-disk-portal/confirm-attach-existing-md.png)

4. <span data-ttu-id="16777-146">Po Azure připojí hello disku toohello virtuálního počítače, je uvedena v nastavení disku hello virtuálního počítače v části **datových disků**.</span><span class="sxs-lookup"><span data-stu-id="16777-146">After Azure attaches hello disk toohello virtual machine, it's listed in hello virtual machine's disk settings under **Data Disks**.</span></span>

## <a name="use-unmanaged-disks"></a><span data-ttu-id="16777-147">Použití nespravovaného disky</span><span class="sxs-lookup"><span data-stu-id="16777-147">Use unmanaged disks</span></span>

### <a name="attach-a-new-disk"></a><span data-ttu-id="16777-148">Připojit nový disk</span><span class="sxs-lookup"><span data-stu-id="16777-148">Attach a new disk</span></span>

1. <span data-ttu-id="16777-149">Na hello **disky** okně klikněte na tlačítko **+ přidat datový disk**.</span><span class="sxs-lookup"><span data-stu-id="16777-149">On hello **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="16777-150">Zkontrolujte hello výchozí nastavení, aktualizovat podle potřeby a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="16777-150">Review hello default settings, update as necessary, and then click **OK**.</span></span>
   
   ![Zkontrolujte nastavení disku](./media/attach-disk-portal/attach-new.png)
3. <span data-ttu-id="16777-152">Jakmile Azure vytvoří hello disk a připojí jej toohello virtuálního počítače, hello nového disku je uvedena v nastavení disku hello virtuálního počítače v části **datových disků**.</span><span class="sxs-lookup"><span data-stu-id="16777-152">After Azure creates hello disk and attaches it toohello virtual machine, hello new disk is listed in hello virtual machine's disk settings under **Data Disks**.</span></span>

### <a name="attach-an-existing-disk"></a><span data-ttu-id="16777-153">Připojení stávajícího disku</span><span class="sxs-lookup"><span data-stu-id="16777-153">Attach an existing disk</span></span>
1. <span data-ttu-id="16777-154">Na hello **disky** okně klikněte na tlačítko **+ přidat datový disk**.</span><span class="sxs-lookup"><span data-stu-id="16777-154">On hello **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="16777-155">V části **připojit stávající disk**, klikněte na tlačítko **souboru virtuálního pevného disku**.</span><span class="sxs-lookup"><span data-stu-id="16777-155">Under **Attach existing disk**, click **VHD File**.</span></span>
   
   ![Připojit stávající disk](./media/attach-disk-portal/attach-existing.png)
3. <span data-ttu-id="16777-157">V části **účty úložiště**, vyberte účet hello a kontejner, který obsahuje soubor VHD hello.</span><span class="sxs-lookup"><span data-stu-id="16777-157">Under **Storage accounts**, select hello account and container that holds hello .vhd file.</span></span>
   
   ![Vyhledejte umístění virtuálního pevného disku](./media/attach-disk-portal/find-storage-container.png)
4. <span data-ttu-id="16777-159">Vyberte soubor VHD hello.</span><span class="sxs-lookup"><span data-stu-id="16777-159">Select hello .vhd file.</span></span>
5. <span data-ttu-id="16777-160">V části **připojit stávající disk**, právě vybraný soubor hello je uveden v části **souboru virtuálního pevného disku**.</span><span class="sxs-lookup"><span data-stu-id="16777-160">Under **Attach existing disk**, hello file you just selected is listed under **VHD File**.</span></span> <span data-ttu-id="16777-161">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="16777-161">Click **OK**.</span></span>
6. <span data-ttu-id="16777-162">Po Azure připojí hello disku toohello virtuálního počítače, je uvedena v nastavení disku hello virtuálního počítače v části **datových disků**.</span><span class="sxs-lookup"><span data-stu-id="16777-162">After Azure attaches hello disk toohello virtual machine, it's listed in hello virtual machine's disk settings under **Data Disks**.</span></span>


## <a name="next-steps"></a><span data-ttu-id="16777-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="16777-163">Next steps</span></span>
<span data-ttu-id="16777-164">Po přidání disku hello potřebujete tooprepare ho.</span><span class="sxs-lookup"><span data-stu-id="16777-164">After hello disk is added, you need tooprepare it for use.</span></span> <span data-ttu-id="16777-165">Další informace najdete v tématu [postupy: Inicializace nový datový disk v systému Linux](add-disk.md).</span><span class="sxs-lookup"><span data-stu-id="16777-165">For more information, see [How to: Initialize a new data disk in Linux](add-disk.md).</span></span>
