---
title: "Vytvoření spravovaného disku z virtuálního pevného disku v Azure | Microsoft Docs"
description: "Vytvoření spravovaného disku z disku VHD, který je aktuálně v účtu úložiště Azure pomocí modelu nasazení Resource Manager."
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
ms.openlocfilehash: c03ebf73f1090b595149daf2eb3e274b05822f4f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-managed-disks-from-unmanaged-disks-in-a-storage-account"></a><span data-ttu-id="1ce79-103">Vytvoření spravované disky z nespravovaných disků v účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="1ce79-103">Create managed disks from unmanaged disks in a storage account</span></span>

<span data-ttu-id="1ce79-104">Spravovaný disk lze vytvořit ze stávající datový disk nebo disk s operačním systémem, který je aktuálně v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="1ce79-104">A managed disk can be created from an existing data disk or an OS disk that is currently in an Azure storage account.</span></span> <span data-ttu-id="1ce79-105">Můžete také vytvořit prázdný disk, který slouží jako nový datový disk pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="1ce79-105">You can also create an empty disk that can be used as a new data disk for a VM.</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="1ce79-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="1ce79-106">Before you begin</span></span>
<span data-ttu-id="1ce79-107">Pokud používáte prostředí PowerShell, ujistěte se, že máte nejnovější verzi modulu prostředí AzureRM.Compute PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1ce79-107">If you use PowerShell, make sure that you have the latest version of the AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="1ce79-108">Spusťte následující příkaz k její instalaci.</span><span class="sxs-lookup"><span data-stu-id="1ce79-108">Run the following command to install it.</span></span>

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="1ce79-109">Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1ce79-109">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="create-a-managed-disk-from-a-vhd-in-an-azure-storage-account"></a><span data-ttu-id="1ce79-110">Vytvoření spravovaného disku z disku VHD v účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="1ce79-110">Create a managed disk from a VHD in an Azure storage account</span></span>

<span data-ttu-id="1ce79-111">V příkladu jsme vytvoření disku z virtuálního pevného disku jako spravovaných disků a přiřaďte ho do parametru **disk 1 $** pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="1ce79-111">In the example we create a disk from a VHD as managed disk and assign it to the parameter **$disk1** to use later.</span></span> 

<span data-ttu-id="1ce79-112">Se vytvoří spravovaných disků **USA – západ** umístění, ve skupině prostředků s názvem **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="1ce79-112">The managed disk will be created in the **West-US** location, in a resource group named **myResourceGroup**.</span></span> <span data-ttu-id="1ce79-113">Na disku budou pojmenované **myDisk** a vytvoří se z virtuálního pevného disku soubor s názvem **myDisk.vhd** jsme předtím nahrála do účtu úložiště s názvem **můj_účet_úložiště**.</span><span class="sxs-lookup"><span data-stu-id="1ce79-113">The disk will be named **myDisk** and it will be created from a VHD file named **myDisk.vhd** we previously uploaded to a storage account named **mystorageaccount**.</span></span> <span data-ttu-id="1ce79-114">Spravovaný disk bude vytvořen v premium místně redundantní úložiště (LRS).</span><span class="sxs-lookup"><span data-stu-id="1ce79-114">The managed disk will be created in premium locally-redundant storage (LRS).</span></span> <span data-ttu-id="1ce79-115">StandardLRS a PremiumLRS jsou jediné **- AccountType** možnosti jsou dostupné pro spravované disky.</span><span class="sxs-lookup"><span data-stu-id="1ce79-115">StandardLRS and PremiumLRS are the only **-AccountType** options available for managed disks.</span></span> 

1.  <span data-ttu-id="1ce79-116">Některé parametry nastavit.</span><span class="sxs-lookup"><span data-stu-id="1ce79-116">Set some parameters</span></span>

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $diskName = "myDisk"
    $vhdUri = "https://mystorageaccount.blob.core.windows.net/vhds/myDisk.vhd"
    ```

2. <span data-ttu-id="1ce79-117">Vytvořte datový disk.</span><span class="sxs-lookup"><span data-stu-id="1ce79-117">Create the data disk.</span></span> 
    ```powershell
    $disk1 = New-AzureRmDisk -DiskName $diskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $vhdUri) -ResourceGroupName $rgName
    ```
    
    

## <a name="create-an-empty-data-disk-as-a-managed-disk"></a><span data-ttu-id="1ce79-118">Vytvoření prázdný datový disk jako spravované disk</span><span class="sxs-lookup"><span data-stu-id="1ce79-118">Create an empty data disk as a managed disk</span></span>

<span data-ttu-id="1ce79-119">V příkladu jsme prázdný datový disk jako spravovaného disku vytvořte a přiřaďte ho do parametru **$dataDisk2** pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="1ce79-119">In the example we create an empty data disk as managed disk and assign it to the parameter **$dataDisk2** to use later.</span></span> <span data-ttu-id="1ce79-120">Prázdný datový disk bude muset být inicializovaného přihlášení k virtuálnímu počítači a pomocí diskmgmt.msc nebo [vzdáleně přes WinRM a skript](attach-disk-ps.md#initialize-the-disk), jakmile je připojen k spuštění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1ce79-120">An empty data disk will need to be initialized logging in to the VM and using diskmgmt.msc or [remotely using WinRM and a script](attach-disk-ps.md#initialize-the-disk), once it is attached to a running VM.</span></span>

<span data-ttu-id="1ce79-121">Prázdný datový disk se vytvoří v **– Západ střední USA** umístění, ve skupině prostředků s názvem **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="1ce79-121">The empty data disk will be created in the **West Central US** location, in a resource group named **myResourceGroup**.</span></span> <span data-ttu-id="1ce79-122">Na disku budou pojmenované **myEmptyDataDisk**.</span><span class="sxs-lookup"><span data-stu-id="1ce79-122">The disk will be named **myEmptyDataDisk**.</span></span> <span data-ttu-id="1ce79-123">Prázdný disk bude vytvořen v premium místně redundantní úložiště (LRS).</span><span class="sxs-lookup"><span data-stu-id="1ce79-123">The empty disk will be created in premium locally-redundant storage (LRS).</span></span> <span data-ttu-id="1ce79-124">StandardLRS a PremiumLRS jsou jediné **- AccountType** možnosti jsou dostupné pro spravované disky.</span><span class="sxs-lookup"><span data-stu-id="1ce79-124">StandardLRS and PremiumLRS are the only **-AccountType** options available for managed disks.</span></span>

<span data-ttu-id="1ce79-125">Velikost disku v tomto příkladu je 128GB, ale měli byste vybrat velikost, která vyhovuje potřebám všechny aplikace běžící na vašem virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="1ce79-125">The disk size in this example is 128GB, but you should choose a size that meets the needs of any applications running on your VM.</span></span>

1.  <span data-ttu-id="1ce79-126">Některé parametry nastavit.</span><span class="sxs-lookup"><span data-stu-id="1ce79-126">Set some parameters</span></span>

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $dataDiskName = "myEmptyDataDisk"
    ```

2. <span data-ttu-id="1ce79-127">Vytvořte datový disk.</span><span class="sxs-lookup"><span data-stu-id="1ce79-127">Create the data disk.</span></span>
    ```powershell
    $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Empty -DiskSizeGB 128) -ResourceGroupName $rgName
    ```
    
## <a name="next-steps"></a><span data-ttu-id="1ce79-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1ce79-128">Next Steps</span></span>   
- <span data-ttu-id="1ce79-129">Pokud již máte virtuální počítač, můžete [připojit datový disk](attach-disk-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1ce79-129">If you already have a VM, you can [attach a data disk](attach-disk-portal.md).</span></span>