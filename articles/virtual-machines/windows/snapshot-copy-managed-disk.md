---
title: "aaaCreate kopii diskem spravované Azure pro zálohování | Microsoft Docs"
description: "Zjistěte, jak toocreate kopii toouse Azure spravované disku pro disk zpět nahoru nebo řešení problémů s problémy."
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
ms.openlocfilehash: 2f33dbbee624bcd813f3c7c3e3401072d0933714
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-copy-of-a-vhd-stored-as-an-azure-managed-disk-by-using-managed-snapshots"></a><span data-ttu-id="e44eb-103">Vytvoření kopie virtuálního pevného disku uložené jako spravované disku Azure s využitím spravované snímky</span><span class="sxs-lookup"><span data-stu-id="e44eb-103">Create a copy of a VHD stored as an Azure Managed Disk by using Managed Snapshots</span></span>
<span data-ttu-id="e44eb-104">Pořízení snímku spravované disku pro zálohu nebo vytvoření disku spravované ze snímku hello a připojte ji tooa testovací virtuální počítač tootroubleshoot.</span><span class="sxs-lookup"><span data-stu-id="e44eb-104">Take a snapshot of a Managed Disk for backup or create a Managed Disk from hello snapshot and attach it tooa test virtual machine tootroubleshoot.</span></span> <span data-ttu-id="e44eb-105">Spravované snímek je úplné v daném okamžiku kopie disku spravovaných virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e44eb-105">A Managed Snapshot is a full point-in-time copy of a VM Managed Disk.</span></span> <span data-ttu-id="e44eb-106">Ji vytvoří kopii svůj disk VHD jen pro čtení a ve výchozím nastavení, ukládá jako standardní spravované Disk.</span><span class="sxs-lookup"><span data-stu-id="e44eb-106">It creates a read-only copy of your VHD and, by default, stores it as a Standard Managed Disk.</span></span> <span data-ttu-id="e44eb-107">Další informace o discích spravovaných najdete v tématu [disky spravované Azure – přehled](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="e44eb-107">For more information about Managed Disks, see [Azure Managed Disks overview](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>

<span data-ttu-id="e44eb-108">Informace o cenách najdete v tématu [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/managed-disks/).</span><span class="sxs-lookup"><span data-stu-id="e44eb-108">For information about pricing, see [Azure Storage Pricing](https://azure.microsoft.com/pricing/details/managed-disks/).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="e44eb-109">Než začnete</span><span class="sxs-lookup"><span data-stu-id="e44eb-109">Before you begin</span></span>
<span data-ttu-id="e44eb-110">Pokud používáte prostředí PowerShell, ujistěte se, že máte nejnovější verzi hello modulu AzureRM.Compute PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="e44eb-110">If you use PowerShell, make sure that you have hello latest version of hello AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="e44eb-111">Spustit hello následující příkaz tooinstall.</span><span class="sxs-lookup"><span data-stu-id="e44eb-111">Run hello following command tooinstall it.</span></span>

```
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="e44eb-112">Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e44eb-112">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>

## <a name="copy-hello-vhd-with-a-snapshot"></a><span data-ttu-id="e44eb-113">Zkopírujte hello virtuálního pevného disku s snímku</span><span class="sxs-lookup"><span data-stu-id="e44eb-113">Copy hello VHD with a snapshot</span></span>
<span data-ttu-id="e44eb-114">Použijte hello portál Azure nebo PowerShell tootake snímek hello spravované disku.</span><span class="sxs-lookup"><span data-stu-id="e44eb-114">Use either hello Azure portal or PowerShell tootake a snapshot of hello Managed Disk.</span></span>

### <a name="use-azure-portal-tootake-a-snapshot"></a><span data-ttu-id="e44eb-115">Použití portálu Azure tootake snímku</span><span class="sxs-lookup"><span data-stu-id="e44eb-115">Use Azure portal tootake a snapshot</span></span> 

1. <span data-ttu-id="e44eb-116">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e44eb-116">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="e44eb-117">Spuštění v levé horní části hello, klikněte na tlačítko **nový** a vyhledejte **snímku**.</span><span class="sxs-lookup"><span data-stu-id="e44eb-117">Starting in hello upper left, click **New** and search for **snapshot**.</span></span>
3. <span data-ttu-id="e44eb-118">V okně hello snímku, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e44eb-118">In hello Snapshot blade, click **Create**.</span></span>
4. <span data-ttu-id="e44eb-119">Zadejte **název** pro vytvoření snímku hello.</span><span class="sxs-lookup"><span data-stu-id="e44eb-119">Enter a **Name** for hello snapshot.</span></span>
5. <span data-ttu-id="e44eb-120">Vyberte existující [skupiny prostředků](../../azure-resource-manager/resource-group-overview.md#resource-groups) nebo název typu hello nové.</span><span class="sxs-lookup"><span data-stu-id="e44eb-120">Select an existing [Resource group](../../azure-resource-manager/resource-group-overview.md#resource-groups) or type hello name for a new one.</span></span> 
6. <span data-ttu-id="e44eb-121">Vyberte umístění datového centra Azure.</span><span class="sxs-lookup"><span data-stu-id="e44eb-121">Select an Azure datacenter Location.</span></span>  
7. <span data-ttu-id="e44eb-122">Pro **zdrojový disk**, vyberte hello toosnapshot spravované disku.</span><span class="sxs-lookup"><span data-stu-id="e44eb-122">For **Source disk**, select hello Managed Disk toosnapshot.</span></span>
8. <span data-ttu-id="e44eb-123">Vyberte hello **typ účtu** toouse toostore hello snímku.</span><span class="sxs-lookup"><span data-stu-id="e44eb-123">Select hello **Account type** toouse toostore hello snapshot.</span></span> <span data-ttu-id="e44eb-124">Doporučujeme, abyste **Standard_LRS** Pokud to uložená na vysokou provádění disku potřebujete.</span><span class="sxs-lookup"><span data-stu-id="e44eb-124">We recommend **Standard_LRS** unless you need it stored on a high performing disk.</span></span>
9. <span data-ttu-id="e44eb-125">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e44eb-125">Click **Create**.</span></span>

### <a name="use-powershell-tootake-a-snapshot"></a><span data-ttu-id="e44eb-126">Pomocí prostředí PowerShell tootake snímku</span><span class="sxs-lookup"><span data-stu-id="e44eb-126">Use PowerShell tootake a snapshot</span></span>
<span data-ttu-id="e44eb-127">Hello následující kroky ukazují, jak tooget hello virtuální pevný disk disku toobe zkopírovat, vytvořte snímek konfigurace hello a pořízení snímku hello disku pomocí rutiny New-AzureRmSnapshot hello<!--Add link toocmdlet when available-->.</span><span class="sxs-lookup"><span data-stu-id="e44eb-127">hello following steps show you how tooget hello VHD disk toobe copied, create hello snapshot configurations, and take a snapshot of hello disk by using hello New-AzureRmSnapshot cmdlet<!--Add link toocmdlet when available-->.</span></span> 

1. <span data-ttu-id="e44eb-128">Nastavte některé parametry.</span><span class="sxs-lookup"><span data-stu-id="e44eb-128">Set some parameters.</span></span> 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$location = 'southeastasia' 
$dataDiskName = 'ContosoMD_datadisk1' 
$snapshotName = 'ContosoMD_datadisk1_snapshot1'  
```
  <span data-ttu-id="e44eb-129">Nahraďte hodnoty parametrů hello:</span><span class="sxs-lookup"><span data-stu-id="e44eb-129">Replace hello parameter values:</span></span>
  -  <span data-ttu-id="e44eb-130">"myResourceGroup" se skupinou prostředků hello Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e44eb-130">"myResourceGroup" with hello VM's resource group.</span></span>
  -  <span data-ttu-id="e44eb-131">"southeastasia" s hello zeměpisné umístění, kam má spravované snímku uloženy.</span><span class="sxs-lookup"><span data-stu-id="e44eb-131">"southeastasia" with hello geographic location where you want your Managed Snapshot stored.</span></span> <!---How do you look these up? -->
  -  <span data-ttu-id="e44eb-132">"ContosoMD_datadisk1" s název hello hello disku VHD, které chcete toocopy.</span><span class="sxs-lookup"><span data-stu-id="e44eb-132">"ContosoMD_datadisk1" with hello name of hello VHD disk that you want toocopy.</span></span>
  -  <span data-ttu-id="e44eb-133">"ContosoMD_datadisk1_snapshot1" s hello, kterou si chcete toouse pro nový snímek hello.</span><span class="sxs-lookup"><span data-stu-id="e44eb-133">"ContosoMD_datadisk1_snapshot1" with hello name you want toouse for hello new snapshot.</span></span>

2. <span data-ttu-id="e44eb-134">Získáte toobe disku VHD hello zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="e44eb-134">Get hello VHD disk toobe copied.</span></span>

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $dataDiskName 
```
3. <span data-ttu-id="e44eb-135">Vytvoření snímku konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="e44eb-135">Create hello snapshot configurations.</span></span> 

 ```powershell
$snapshot =  New-AzureRmSnapshotConfig -SourceUri $disk.Id -CreateOption Copy -Location $location 
```
4. <span data-ttu-id="e44eb-136">Pořízení snímku hello.</span><span class="sxs-lookup"><span data-stu-id="e44eb-136">Take hello snapshot.</span></span>

 ```powershell
New-AzureRmSnapshot -Snapshot $snapshot -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName 
```
<span data-ttu-id="e44eb-137">Pokud plánování toouse hello snímku toocreate Disk spravované a připojte ji virtuální počítač, který potřebuje toobe vysoké provádění, použijte parametr hello `-AccountType Premium_LRS` pomocí příkazu New-AzureRmSnapshot hello.</span><span class="sxs-lookup"><span data-stu-id="e44eb-137">If you plan toouse hello snapshot toocreate a Managed Disk and attach it a VM that needs toobe high performing, use hello parameter `-AccountType Premium_LRS` with hello New-AzureRmSnapshot command.</span></span> <span data-ttu-id="e44eb-138">Parametr Hello vytvoří snímek hello tak, aby je uložena jako spravovaný Disk úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="e44eb-138">hello parameter creates hello snapshot so that it's stored as a Premium Managed Disk.</span></span> <span data-ttu-id="e44eb-139">Premium spravované disky jsou dražší než Standard.</span><span class="sxs-lookup"><span data-stu-id="e44eb-139">Premium Managed Disks are more expensive than Standard.</span></span> <span data-ttu-id="e44eb-140">Proto ujistěte se, že je skutečně potřebujete Premium před použitím tohoto parametru.</span><span class="sxs-lookup"><span data-stu-id="e44eb-140">So be sure you really need Premium before using that parameter.</span></span>


