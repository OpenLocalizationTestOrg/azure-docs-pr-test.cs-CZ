---
title: "Vytvořit kopii diskem spravované Azure pro back up | Microsoft Docs"
description: "Naučte se vytvořit kopii Azure spravované disku pro back up nebo řešení potíží s disku."
documentationcenter: 
author: cwatson-cat
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 15eb778e-fc07-45ef-bdc8-9090193a6d20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 2/9/2017
ms.author: cwatson
ms.openlocfilehash: a7527b12f4f0d2b45713a0c0109d81ff51293fd8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-copy-of-a-vhd-stored-as-an-azure-managed-disk-by-using-managed-snapshots"></a><span data-ttu-id="9d241-103">Vytvoření kopie virtuálního pevného disku uložené jako spravované disku Azure s využitím spravované snímky</span><span class="sxs-lookup"><span data-stu-id="9d241-103">Create a copy of a VHD stored as an Azure Managed Disk by using Managed Snapshots</span></span>
<span data-ttu-id="9d241-104">Pořízení snímku spravované disku pro zálohu nebo vytvoření disku spravované ze snímku a jeho připojení k testovací virtuální počítač k řešení.</span><span class="sxs-lookup"><span data-stu-id="9d241-104">Take a snapshot of a Managed Disk for backup or create a Managed Disk from the snapshot and attach it to a test virtual machine to troubleshoot.</span></span> <span data-ttu-id="9d241-105">Spravované snímek je úplné v daném okamžiku kopie disku spravovaných virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9d241-105">A Managed Snapshot is a full point-in-time copy of a VM Managed Disk.</span></span> <span data-ttu-id="9d241-106">Ji vytvoří kopii svůj disk VHD jen pro čtení a ve výchozím nastavení, ukládá jako standardní spravované Disk.</span><span class="sxs-lookup"><span data-stu-id="9d241-106">It creates a read-only copy of your VHD and, by default, stores it as a Standard Managed Disk.</span></span> <span data-ttu-id="9d241-107">Další informace o discích spravovaných najdete v tématu [disky spravované Azure – přehled](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="9d241-107">For more information about Managed Disks, see [Azure Managed Disks overview](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>

<span data-ttu-id="9d241-108">Informace o cenách najdete v tématu [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/managed-disks/).</span><span class="sxs-lookup"><span data-stu-id="9d241-108">For information about pricing, see [Azure Storage Pricing](https://azure.microsoft.com/pricing/details/managed-disks/).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="9d241-109">Než začnete</span><span class="sxs-lookup"><span data-stu-id="9d241-109">Before you begin</span></span>
<span data-ttu-id="9d241-110">Pokud používáte prostředí PowerShell, ujistěte se, že máte nejnovější verzi modulu prostředí AzureRM.Compute PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9d241-110">If you use PowerShell, make sure that you have the latest version of the AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="9d241-111">Spusťte následující příkaz k její instalaci.</span><span class="sxs-lookup"><span data-stu-id="9d241-111">Run the following command to install it.</span></span>

```
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="9d241-112">Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9d241-112">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>

## <a name="copy-the-vhd-with-a-snapshot"></a><span data-ttu-id="9d241-113">Zkopírujte virtuální pevný disk s snímku</span><span class="sxs-lookup"><span data-stu-id="9d241-113">Copy the VHD with a snapshot</span></span>
<span data-ttu-id="9d241-114">K pořízení snímku na Disk spravovaný pomocí portálu Azure nebo prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9d241-114">Use either the Azure portal or PowerShell to take a snapshot of the Managed Disk.</span></span>

### <a name="use-azure-portal-to-take-a-snapshot"></a><span data-ttu-id="9d241-115">Pořízení snímku pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9d241-115">Use Azure portal to take a snapshot</span></span> 

1. <span data-ttu-id="9d241-116">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9d241-116">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="9d241-117">Spouštění v levém horním, klikněte na tlačítko **nový** a vyhledejte **snímku**.</span><span class="sxs-lookup"><span data-stu-id="9d241-117">Starting in the upper left, click **New** and search for **snapshot**.</span></span>
3. <span data-ttu-id="9d241-118">V okně snímku, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9d241-118">In the Snapshot blade, click **Create**.</span></span>
4. <span data-ttu-id="9d241-119">Zadejte **název** pro snímku.</span><span class="sxs-lookup"><span data-stu-id="9d241-119">Enter a **Name** for the snapshot.</span></span>
5. <span data-ttu-id="9d241-120">Vyberte existující [skupinu prostředků](../../azure-resource-manager/resource-group-overview.md#resource-groups) nebo zadejte název nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="9d241-120">Select an existing [Resource group](../../azure-resource-manager/resource-group-overview.md#resource-groups) or type the name for a new one.</span></span> 
6. <span data-ttu-id="9d241-121">Vyberte umístění datového centra Azure.</span><span class="sxs-lookup"><span data-stu-id="9d241-121">Select an Azure datacenter Location.</span></span>  
7. <span data-ttu-id="9d241-122">Pro **zdrojový disk**, vyberte spravované Disk snímek.</span><span class="sxs-lookup"><span data-stu-id="9d241-122">For **Source disk**, select the Managed Disk to snapshot.</span></span>
8. <span data-ttu-id="9d241-123">Vyberte **typ účtu** používat k uložení snímku.</span><span class="sxs-lookup"><span data-stu-id="9d241-123">Select the **Account type** to use to store the snapshot.</span></span> <span data-ttu-id="9d241-124">Doporučujeme, abyste **Standard_LRS** Pokud to uložená na vysokou provádění disku potřebujete.</span><span class="sxs-lookup"><span data-stu-id="9d241-124">We recommend **Standard_LRS** unless you need it stored on a high performing disk.</span></span>
9. <span data-ttu-id="9d241-125">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9d241-125">Click **Create**.</span></span>

### <a name="use-powershell-to-take-a-snapshot"></a><span data-ttu-id="9d241-126">Pořízení snímku pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="9d241-126">Use PowerShell to take a snapshot</span></span>
<span data-ttu-id="9d241-127">Následující kroky vám ukážou, jak získat disku VHD zkopírovat, vytvořte snímek konfigurace, a pořízení snímku disku pomocí rutiny New-AzureRmSnapshot<!--Add link to cmdlet when available-->.</span><span class="sxs-lookup"><span data-stu-id="9d241-127">The following steps show you how to get the VHD disk to be copied, create the snapshot configurations, and take a snapshot of the disk by using the New-AzureRmSnapshot cmdlet<!--Add link to cmdlet when available-->.</span></span> 

1. <span data-ttu-id="9d241-128">Nastavte některé parametry.</span><span class="sxs-lookup"><span data-stu-id="9d241-128">Set some parameters.</span></span> 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$location = 'southeastasia' 
$dataDiskName = 'ContosoMD_datadisk1' 
$snapshotName = 'ContosoMD_datadisk1_snapshot1'  
```
  <span data-ttu-id="9d241-129">Nahraďte hodnoty parametrů:</span><span class="sxs-lookup"><span data-stu-id="9d241-129">Replace the parameter values:</span></span>
  -  <span data-ttu-id="9d241-130">"myResourceGroup" se skupinou prostředků Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9d241-130">"myResourceGroup" with the VM's resource group.</span></span>
  -  <span data-ttu-id="9d241-131">"southeastasia" s zeměpisného umístění, kam chcete spravovat snímku uloženy.</span><span class="sxs-lookup"><span data-stu-id="9d241-131">"southeastasia" with the geographic location where you want your Managed Snapshot stored.</span></span> <!---How do you look these up? -->
  -  <span data-ttu-id="9d241-132">"ContosoMD_datadisk1" s název disku VHD, který chcete zkopírovat.</span><span class="sxs-lookup"><span data-stu-id="9d241-132">"ContosoMD_datadisk1" with the name of the VHD disk that you want to copy.</span></span>
  -  <span data-ttu-id="9d241-133">"ContosoMD_datadisk1_snapshot1" s název, který chcete použít pro nový snímek.</span><span class="sxs-lookup"><span data-stu-id="9d241-133">"ContosoMD_datadisk1_snapshot1" with the name you want to use for the new snapshot.</span></span>

2. <span data-ttu-id="9d241-134">Získáte virtuální pevný disk disku ke kopírování.</span><span class="sxs-lookup"><span data-stu-id="9d241-134">Get the VHD disk to be copied.</span></span>

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $dataDiskName 
```
3. <span data-ttu-id="9d241-135">Vytvořte snímek konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9d241-135">Create the snapshot configurations.</span></span> 

 ```powershell
$snapshot =  New-AzureRmSnapshotConfig -SourceUri $disk.Id -CreateOption Copy -Location $location 
```
4. <span data-ttu-id="9d241-136">Vytvořte snímek.</span><span class="sxs-lookup"><span data-stu-id="9d241-136">Take the snapshot.</span></span>

 ```powershell
New-AzureRmSnapshot -Snapshot $snapshot -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName 
```
<span data-ttu-id="9d241-137">Pokud máte v plánu používat k vytvoření disku spravované a připojte ji virtuální počítač, který musí být vysoké provádění snímku, použijte parametr `-AccountType Premium_LRS` pomocí příkazu New-AzureRmSnapshot.</span><span class="sxs-lookup"><span data-stu-id="9d241-137">If you plan to use the snapshot to create a Managed Disk and attach it a VM that needs to be high performing, use the parameter `-AccountType Premium_LRS` with the New-AzureRmSnapshot command.</span></span> <span data-ttu-id="9d241-138">Parametr vytvoří snímek tak, aby je uložena jako spravovaný Disk úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="9d241-138">The parameter creates the snapshot so that it's stored as a Premium Managed Disk.</span></span> <span data-ttu-id="9d241-139">Premium spravované disky jsou dražší než Standard.</span><span class="sxs-lookup"><span data-stu-id="9d241-139">Premium Managed Disks are more expensive than Standard.</span></span> <span data-ttu-id="9d241-140">Proto ujistěte se, že je skutečně potřebujete Premium před použitím tohoto parametru.</span><span class="sxs-lookup"><span data-stu-id="9d241-140">So be sure you really need Premium before using that parameter.</span></span>


