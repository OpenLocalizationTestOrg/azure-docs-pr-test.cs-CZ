---
title: "aaaCreate se spravovaným diskem z disku VHD v Azure | Microsoft Docs"
description: "Vytvoření spravovaného disku z disku VHD, který je aktuálně v účtu úložiště Azure pomocí modelu nasazení Resource Manager hello."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/05/2017
ms.author: cynthn
ms.openlocfilehash: 77adaac5419186ff85039fe2c4752f021aa5e448
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-managed-disks-from-unmanaged-disks-in-a-storage-account"></a><span data-ttu-id="6d7df-103">Vytvoření spravované disky z nespravovaných disků v účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="6d7df-103">Create managed disks from unmanaged disks in a storage account</span></span>

<span data-ttu-id="6d7df-104">Spravovaný disk lze vytvořit ze stávající datový disk nebo disk s operačním systémem, který je aktuálně v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="6d7df-104">A managed disk can be created from an existing data disk or an OS disk that is currently in an Azure storage account.</span></span> <span data-ttu-id="6d7df-105">Můžete také vytvořit prázdný disk, který slouží jako nový datový disk pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6d7df-105">You can also create an empty disk that can be used as a new data disk for a VM.</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="6d7df-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="6d7df-106">Before you begin</span></span>
<span data-ttu-id="6d7df-107">Pokud používáte prostředí PowerShell, ujistěte se, že máte nejnovější verzi hello modulu AzureRM.Compute PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="6d7df-107">If you use PowerShell, make sure that you have hello latest version of hello AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="6d7df-108">Spustit hello následující příkaz tooinstall.</span><span class="sxs-lookup"><span data-stu-id="6d7df-108">Run hello following command tooinstall it.</span></span>

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="6d7df-109">Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6d7df-109">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="create-a-managed-disk-from-a-vhd-in-an-azure-storage-account"></a><span data-ttu-id="6d7df-110">Vytvoření spravovaného disku z disku VHD v účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="6d7df-110">Create a managed disk from a VHD in an Azure storage account</span></span>

<span data-ttu-id="6d7df-111">V příkladu hello jsme vytvoření disku z virtuálního pevného disku jako spravovaných disků a přiřaďte ho toohello parametr **$ disk 1** toouse později.</span><span class="sxs-lookup"><span data-stu-id="6d7df-111">In hello example we create a disk from a VHD as managed disk and assign it toohello parameter **$disk1** toouse later.</span></span> 

<span data-ttu-id="6d7df-112">Hello spravovaného disku budou vytvořeny v hello **USA – západ** umístění, ve skupině prostředků s názvem **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="6d7df-112">hello managed disk will be created in hello **West-US** location, in a resource group named **myResourceGroup**.</span></span> <span data-ttu-id="6d7df-113">bude mít název disku Hello **myDisk** a vytvoří se z virtuálního pevného disku soubor s názvem **myDisk.vhd** jsme předtím nahrála tooa účet úložiště s názvem **můj_účet_úložiště**.</span><span class="sxs-lookup"><span data-stu-id="6d7df-113">hello disk will be named **myDisk** and it will be created from a VHD file named **myDisk.vhd** we previously uploaded tooa storage account named **mystorageaccount**.</span></span> <span data-ttu-id="6d7df-114">místně redundantní úložiště (LRS) úrovně premium vytvoří spravovaných disků na Hello.</span><span class="sxs-lookup"><span data-stu-id="6d7df-114">hello managed disk will be created in premium locally-redundant storage (LRS).</span></span> <span data-ttu-id="6d7df-115">StandardLRS a PremiumLRS jsou pouze hello **- AccountType** možnosti jsou dostupné pro spravované disky.</span><span class="sxs-lookup"><span data-stu-id="6d7df-115">StandardLRS and PremiumLRS are hello only **-AccountType** options available for managed disks.</span></span> 

1.  <span data-ttu-id="6d7df-116">Některé parametry nastavit.</span><span class="sxs-lookup"><span data-stu-id="6d7df-116">Set some parameters</span></span>

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $diskName = "myDisk"
    $vhdUri = "https://mystorageaccount.blob.core.windows.net/vhds/myDisk.vhd"
    ```

2. <span data-ttu-id="6d7df-117">Vytvořte hello datový disk.</span><span class="sxs-lookup"><span data-stu-id="6d7df-117">Create hello data disk.</span></span> 
    ```powershell
    $disk1 = New-AzureRmDisk -DiskName $diskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $vhdUri) -ResourceGroupName $rgName
    ```
    
    

## <a name="create-an-empty-data-disk-as-a-managed-disk"></a><span data-ttu-id="6d7df-118">Vytvoření prázdný datový disk jako spravované disk</span><span class="sxs-lookup"><span data-stu-id="6d7df-118">Create an empty data disk as a managed disk</span></span>

<span data-ttu-id="6d7df-119">V příkladu hello jsme vytvořte prázdný datový disk jako spravovaných disků a přiřaďte ho toohello parametr **$dataDisk2** toouse později.</span><span class="sxs-lookup"><span data-stu-id="6d7df-119">In hello example we create an empty data disk as managed disk and assign it toohello parameter **$dataDisk2** toouse later.</span></span> <span data-ttu-id="6d7df-120">Prázdný datový disk bude nutné toobe inicializovat protokolování v toohello virtuálních počítačů a pomocí diskmgmt.msc nebo [vzdáleně přes WinRM a skript](attach-disk-ps.md#initialize-the-disk), až bude připojený tooa spuštění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6d7df-120">An empty data disk will need toobe initialized logging in toohello VM and using diskmgmt.msc or [remotely using WinRM and a script](attach-disk-ps.md#initialize-the-disk), once it is attached tooa running VM.</span></span>

<span data-ttu-id="6d7df-121">Hello prázdný datový disk se vytvoří hello **– Západ střední USA** umístění, ve skupině prostředků s názvem **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="6d7df-121">hello empty data disk will be created in hello **West Central US** location, in a resource group named **myResourceGroup**.</span></span> <span data-ttu-id="6d7df-122">bude mít název disku Hello **myEmptyDataDisk**.</span><span class="sxs-lookup"><span data-stu-id="6d7df-122">hello disk will be named **myEmptyDataDisk**.</span></span> <span data-ttu-id="6d7df-123">prázdný disk Hello budou vytvořeny v premium místně redundantní úložiště (LRS).</span><span class="sxs-lookup"><span data-stu-id="6d7df-123">hello empty disk will be created in premium locally-redundant storage (LRS).</span></span> <span data-ttu-id="6d7df-124">StandardLRS a PremiumLRS jsou pouze hello **- AccountType** možnosti jsou dostupné pro spravované disky.</span><span class="sxs-lookup"><span data-stu-id="6d7df-124">StandardLRS and PremiumLRS are hello only **-AccountType** options available for managed disks.</span></span>

<span data-ttu-id="6d7df-125">velikost disku Hello v tomto příkladu je 128GB, ale měli byste vybrat velikost, která vyhovuje potřebám hello všech aplikací běžících na vašem virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="6d7df-125">hello disk size in this example is 128GB, but you should choose a size that meets hello needs of any applications running on your VM.</span></span>

1.  <span data-ttu-id="6d7df-126">Některé parametry nastavit.</span><span class="sxs-lookup"><span data-stu-id="6d7df-126">Set some parameters</span></span>

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $dataDiskName = "myEmptyDataDisk"
    ```

2. <span data-ttu-id="6d7df-127">Vytvořte hello datový disk.</span><span class="sxs-lookup"><span data-stu-id="6d7df-127">Create hello data disk.</span></span>
    ```powershell
    $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Empty -DiskSizeGB 128) -ResourceGroupName $rgName
    ```
    
## <a name="next-steps"></a><span data-ttu-id="6d7df-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6d7df-128">Next Steps</span></span>   
- <span data-ttu-id="6d7df-129">Pokud již máte virtuální počítač, můžete [připojit datový disk](attach-disk-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6d7df-129">If you already have a VM, you can [attach a data disk](attach-disk-portal.md).</span></span>
