---
title: "Vytvoření virtuálního počítače s Windows z specializované virtuálního pevného disku v Azure | Microsoft Docs"
description: "Vytvořte nový virtuální počítač Windows připojením specializované spravované disk jako disk operačního systému, použití v modelu nasazení Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3b7d3cd5-e3d7-4041-a2a7-0290447458ea
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: cynthn
ms.openlocfilehash: fa952286a9ceca8b3b2c7efe2cc4867a2728c477
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-windows-vm-from-a-specialized-disk"></a><span data-ttu-id="cbb18-103">Vytvoření virtuálního počítače s Windows z specializované disku</span><span class="sxs-lookup"><span data-stu-id="cbb18-103">Create a Windows VM from a specialized disk</span></span>

<span data-ttu-id="cbb18-104">Vytvoření nového virtuálního počítače připojením specializované spravované disk jako disk operačního systému pomocí prostředí Powershell.</span><span class="sxs-lookup"><span data-stu-id="cbb18-104">Create a new VM by attaching a specialized managed disk as the OS disk using Powershell.</span></span> <span data-ttu-id="cbb18-105">Specializované disk je kopie virtuálního pevného disku (VHD) ze stávajícího virtuálního počítače, který uchovává uživatelské účty, aplikace a další data o stavu z vašeho původního virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cbb18-105">A specialized disk is a copy of virtual hard disk (VHD) from an existing VM that maintains the user accounts, applications, and other state data from your original VM.</span></span> 

<span data-ttu-id="cbb18-106">Pokud použijete specializované virtuálního pevného disku pro vytvoření nového virtuálního počítače, nový virtuální počítač zachová název počítače původní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="cbb18-106">When you use a specialized VHD to create a new VM, the new VM retains the computer name of the original VM.</span></span> <span data-ttu-id="cbb18-107">Další informace specifické pro počítač se také nacházet a v některých případech tyto duplicitní informace může způsobit problémy.</span><span class="sxs-lookup"><span data-stu-id="cbb18-107">Other computer-specific information is also be kept and, in some cases, this duplicate information could cause issues.</span></span> <span data-ttu-id="cbb18-108">Být vědět, jaké typy informace specifické pro počítač aplikace se spoléhají na při kopírování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cbb18-108">Be aware of what types of computer-specific information your applications rely on when copying a VM.</span></span>

<span data-ttu-id="cbb18-109">Máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="cbb18-109">You have two options:</span></span>
* [<span data-ttu-id="cbb18-110">Nahrání virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="cbb18-110">Upload a VHD</span></span>](#option-1-upload-a-specialized-vhd)
* [<span data-ttu-id="cbb18-111">Zkopírujte existující virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="cbb18-111">Copy an existing Azure VM</span></span>](#option-2-copy-an-existing-azure-vm)

<span data-ttu-id="cbb18-112">Toto téma ukazuje, jak používat spravovaného disky.</span><span class="sxs-lookup"><span data-stu-id="cbb18-112">This topic shows you how to use managed disks.</span></span> <span data-ttu-id="cbb18-113">Pokud máte starší verzi nasazení, vyžaduje použití účtu úložiště najdete v tématu [vytvoření virtuálního počítače z specializované virtuálního pevného disku v účtu úložiště](sa-create-vm-specialized.md)</span><span class="sxs-lookup"><span data-stu-id="cbb18-113">If you have a legacy deployment that requires using a storage account, see [Create a VM from a specialized VHD in a storage account](sa-create-vm-specialized.md)</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="cbb18-114">Než začnete</span><span class="sxs-lookup"><span data-stu-id="cbb18-114">Before you begin</span></span>
<span data-ttu-id="cbb18-115">Pokud používáte prostředí PowerShell, ujistěte se, že máte nejnovější verzi modulu prostředí AzureRM.Compute PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cbb18-115">If you use PowerShell, make sure that you have the latest version of the AzureRM.Compute PowerShell module.</span></span> 

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="cbb18-116">Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cbb18-116">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="option-1-upload-a-specialized-vhd"></a><span data-ttu-id="cbb18-117">Možnost 1: Nahrát specializované virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="cbb18-117">Option 1: Upload a specialized VHD</span></span>

<span data-ttu-id="cbb18-118">Můžete nahrát virtuální pevný disk z virtuálního počítače specializované vytvořené pomocí místní virtualizace nástroje, jako je technologie Hyper-V nebo na virtuální počítač exportovat z jiného cloudu.</span><span class="sxs-lookup"><span data-stu-id="cbb18-118">You can upload the VHD from a specialized VM created with an on-premises virtualization tool, like Hyper-V, or a VM exported from another cloud.</span></span>

### <a name="prepare-the-vm"></a><span data-ttu-id="cbb18-119">Příprava virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cbb18-119">Prepare the VM</span></span>
<span data-ttu-id="cbb18-120">Pokud máte v úmyslu použít virtuální pevný disk jako-je chcete vytvořit nový virtuální počítač, zkontrolujte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="cbb18-120">If you intend to use the VHD as-is to create a new VM, ensure the following steps are completed.</span></span> 
  
  * <span data-ttu-id="cbb18-121">[Příprava virtuálního pevného disku Windows nahrát do Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cbb18-121">[Prepare a Windows VHD to upload to Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="cbb18-122">**Nechcete** generalize virtuální počítač pomocí nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="cbb18-122">**Do not** generalize the VM using Sysprep.</span></span>
  * <span data-ttu-id="cbb18-123">Odeberte všechny hosta virtualizačních nástrojů a agentů, které jsou nainstalovány na virtuálním počítači (např. nástroje VMware).</span><span class="sxs-lookup"><span data-stu-id="cbb18-123">Remove any guest virtualization tools and agents that are installed on the VM (like VMware tools).</span></span>
  * <span data-ttu-id="cbb18-124">Zajistěte, aby že virtuální počítač nakonfigurovaný tak, aby jeho IP adresu a nastavení DNS pomocí protokolu DHCP pro vyžádání obsahu.</span><span class="sxs-lookup"><span data-stu-id="cbb18-124">Ensure the VM is configured to pull its IP address and DNS settings via DHCP.</span></span> <span data-ttu-id="cbb18-125">To zajistí, že server získá IP adresu v rámci virtuální sítě, při spuštění.</span><span class="sxs-lookup"><span data-stu-id="cbb18-125">This ensures that the server obtains an IP address within the VNet when it starts up.</span></span> 


### <a name="get-the-storage-account"></a><span data-ttu-id="cbb18-126">Získat účet úložiště</span><span class="sxs-lookup"><span data-stu-id="cbb18-126">Get the storage account</span></span>
<span data-ttu-id="cbb18-127">Budete potřebovat účet úložiště v Azure k ukládání nahraný virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="cbb18-127">You need a storage account in Azure to store the uploaded VHD.</span></span> <span data-ttu-id="cbb18-128">Můžete použít existující účet úložiště, nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="cbb18-128">You can either use an existing storage account or create a new one.</span></span> 

<span data-ttu-id="cbb18-129">Chcete-li zobrazit účty úložiště k dispozici, zadejte:</span><span class="sxs-lookup"><span data-stu-id="cbb18-129">To show the available storage accounts, type:</span></span>

```powershell
Get-AzureRmStorageAccount
```

<span data-ttu-id="cbb18-130">Pokud chcete použít existující účet úložiště, pokračujte [nahrávat VHD](#upload-the-vhd-to-your-storage-account) části.</span><span class="sxs-lookup"><span data-stu-id="cbb18-130">If you want to use an existing storage account, proceed to the [Upload the VHD](#upload-the-vhd-to-your-storage-account) section.</span></span>

<span data-ttu-id="cbb18-131">Pokud potřebujete vytvořit účet úložiště, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="cbb18-131">If you need to create a storage account, follow these steps:</span></span>

1. <span data-ttu-id="cbb18-132">Je třeba na název skupiny prostředků, kde by měl být vytvořen účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="cbb18-132">You need the name of the resource group where the storage account should be created.</span></span> <span data-ttu-id="cbb18-133">Chcete-li zjistit všechny skupiny prostředků, které jsou v rámci vašeho předplatného, zadejte:</span><span class="sxs-lookup"><span data-stu-id="cbb18-133">To find out all the resource groups that are in your subscription, type:</span></span>
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    <span data-ttu-id="cbb18-134">Chcete-li vytvořit skupinu prostředků s názvem *myResourceGroup* v *západní USA* oblast, zadejte:</span><span class="sxs-lookup"><span data-stu-id="cbb18-134">To create a resource group named *myResourceGroup* in the *West US* region, type:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. <span data-ttu-id="cbb18-135">Vytvořit účet úložiště s názvem *můj_účet_úložiště* v této skupině prostředků s použitím [AzureRmStorageAccount nový](/powershell/module/azurerm.storage/new-azurermstorageaccount) rutiny:</span><span class="sxs-lookup"><span data-stu-id="cbb18-135">Create a storage account named *mystorageaccount* in this resource group by using the [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span></span>
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```

### <a name="upload-the-vhd-to-your-storage-account"></a><span data-ttu-id="cbb18-136">Nahrání virtuálního pevného disku do účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="cbb18-136">Upload the VHD to your storage account</span></span> 
<span data-ttu-id="cbb18-137">Použití [přidat AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) rutiny odeslání disku VHD do kontejneru v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="cbb18-137">Use the [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet to upload the VHD to a container in your storage account.</span></span> <span data-ttu-id="cbb18-138">Tento příklad nahrávání souboru *myVHD.vhd* z `"C:\Users\Public\Documents\Virtual hard disks\"` na účet úložiště s názvem *můj_účet_úložiště* v *myResourceGroup* skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="cbb18-138">This example uploads the file *myVHD.vhd* from `"C:\Users\Public\Documents\Virtual hard disks\"` to a storage account named *mystorageaccount* in the *myResourceGroup* resource group.</span></span> <span data-ttu-id="cbb18-139">Soubor je uložen v kontejneru s názvem *můj_kontejner* a nový název souboru bude *myUploadedVHD.vhd*.</span><span class="sxs-lookup"><span data-stu-id="cbb18-139">The file is stored in the container named *mycontainer* and the new file name will be *myUploadedVHD.vhd*.</span></span>

```powershell
$resourceGroupName = "myResourceGroup"
$urlOfUploadedVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $resourceGroupName -Destination $urlOfUploadedVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


<span data-ttu-id="cbb18-140">Pokud bylo úspěšné, můžete získat odpovědi, která vypadá podobně jako tento:</span><span class="sxs-lookup"><span data-stu-id="cbb18-140">If successful, you get a response that looks similar to this:</span></span>

```powershell
MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for the operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

<span data-ttu-id="cbb18-141">V závislosti na připojení k síti a velikost souboru virtuálního pevného disku tento příkaz může trvat nějakou dobu pro dokončení</span><span class="sxs-lookup"><span data-stu-id="cbb18-141">Depending on your network connection and the size of your VHD file, this command may take a while to complete</span></span>

### <a name="create-a-managed-disk-from-the-vhd"></a><span data-ttu-id="cbb18-142">Vytvoření spravovaného disku z virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="cbb18-142">Create a managed disk from the VHD</span></span>

<span data-ttu-id="cbb18-143">Vytvoření spravovaného disku z specializované virtuálního pevného disku v účtu úložiště pomocí [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk).</span><span class="sxs-lookup"><span data-stu-id="cbb18-143">Create a managed disk from the specialized VHD in your storage account using [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk).</span></span> <span data-ttu-id="cbb18-144">Tento příklad používá **myOSDisk1** pro název disku vloží disku *StandardLRS* úložiště a používá *https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd* jako identifikátor URI pro zdrojové virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="cbb18-144">This example uses **myOSDisk1** for the disk name, puts the disk in *StandardLRS* storage, and uses *https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd* as the URI for the source VHD.</span></span>

<span data-ttu-id="cbb18-145">Vytvořte novou skupinu prostředků pro nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="cbb18-145">Create a new resource group for the new VM.</span></span>

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location -Name $destinationResourceGroup
```

<span data-ttu-id="cbb18-146">Vytvořte nový disk operačního systému z nahraný virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="cbb18-146">Create the new OS disk from the uploaded VHD.</span></span> 

```powershell
$sourceUri = https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd)
$osDiskName = 'myOsDisk'
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig -AccountType StandardLRS  -Location $location -CreateOption Import `
    -SourceUri $sourceUri) `
    -ResourceGroupName $destinationResourceGroup
```

## <a name="option-2-copy-an-existing-azure-vm"></a><span data-ttu-id="cbb18-147">Možnost 2: Zkopírujte existující virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="cbb18-147">Option 2: Copy an existing Azure VM</span></span>

<span data-ttu-id="cbb18-148">Můžete vytvořit kopii virtuálního počítače, že používá spravuje disky pořízení snímku virtuálního počítače, a potom pomocí tohoto snímku pro vytvoření nového spravovaného disku a vytvoření nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cbb18-148">You can create a copy of a VM that uses managed disks by taking a snapshot of the VM, then using that snapshot to create a new managed disk and a new VM.</span></span>


### <a name="take-a-snapshot-of-the-os-disk"></a><span data-ttu-id="cbb18-149">Pořízení snímku disk operačního systému</span><span class="sxs-lookup"><span data-stu-id="cbb18-149">Take a snapshot of the OS disk</span></span>

<span data-ttu-id="cbb18-150">Můžete provést z snímek a celý virtuální počítač (včetně všech disků) nebo právě jeden disk.</span><span class="sxs-lookup"><span data-stu-id="cbb18-150">You can take a snapshot of and entire VM (including all disks) or of just a single disk.</span></span> <span data-ttu-id="cbb18-151">Následující kroky ukazují, jak k pořízení snímku právě disk operačního systému z virtuální počítač pomocí [New-AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) rutiny.</span><span class="sxs-lookup"><span data-stu-id="cbb18-151">The following steps show you how to take a snapshot of just the OS disk of your VM using the [New-AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) cmdlet.</span></span> 

<span data-ttu-id="cbb18-152">Nastavte některé parametry.</span><span class="sxs-lookup"><span data-stu-id="cbb18-152">Set some parameters.</span></span> 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$vmName = 'myVM'
$location = 'westus' 
$snapshotName = 'mySnapshot'  
```

<span data-ttu-id="cbb18-153">Získejte objekt virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cbb18-153">Get the VM object.</span></span>

```powershell
$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $resourceGroupName
```
<span data-ttu-id="cbb18-154">Získáte název disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="cbb18-154">Get the OS disk name.</span></span>

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $vm.StorageProfile.OsDisk.Name
```

<span data-ttu-id="cbb18-155">Vytvoření snímku konfigurace.</span><span class="sxs-lookup"><span data-stu-id="cbb18-155">Create the snapshot configuration.</span></span> 

 ```powershell
$snapshotConfig =  New-AzureRmSnapshotConfig -SourceUri $disk.Id -OsType Windows -CreateOption Copy -Location $location 
```

<span data-ttu-id="cbb18-156">Vytvořte snímek.</span><span class="sxs-lookup"><span data-stu-id="cbb18-156">Take the snapshot.</span></span>

```powershell
$snapShot = New-AzureRmSnapshot -Snapshot $snapshotConfig -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName
```


<span data-ttu-id="cbb18-157">Pokud máte v úmyslu vytvořit virtuální počítač, který musí být vysoké provádění pomocí snímku, použijte parametr `-AccountType Premium_LRS` pomocí příkazu New-AzureRmSnapshot.</span><span class="sxs-lookup"><span data-stu-id="cbb18-157">If you plan to use the snapshot to create a VM that needs to be high performing, use the parameter `-AccountType Premium_LRS` with the New-AzureRmSnapshot command.</span></span> <span data-ttu-id="cbb18-158">Parametr vytvoří snímek tak, aby je uložena jako spravovaný Disk úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="cbb18-158">The parameter creates the snapshot so that it's stored as a Premium Managed Disk.</span></span> <span data-ttu-id="cbb18-159">Premium spravované disky jsou dražší než Standard.</span><span class="sxs-lookup"><span data-stu-id="cbb18-159">Premium Managed Disks are more expensive than Standard.</span></span> <span data-ttu-id="cbb18-160">Proto ujistěte se, že je skutečně potřebujete Premium před použitím parametru.</span><span class="sxs-lookup"><span data-stu-id="cbb18-160">So be sure you really need Premium before using the parameter.</span></span>

### <a name="create-a-new-disk-from-the-snapshot"></a><span data-ttu-id="cbb18-161">Vytvoření nového disku ze snímku</span><span class="sxs-lookup"><span data-stu-id="cbb18-161">Create a new disk from the snapshot</span></span>

<span data-ttu-id="cbb18-162">Vytvoření spravovaného disku pomocí snímku [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk).</span><span class="sxs-lookup"><span data-stu-id="cbb18-162">Create a managed disk from the snapshot using [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk).</span></span> <span data-ttu-id="cbb18-163">Tento příklad používá *myOSDisk* pro název disku.</span><span class="sxs-lookup"><span data-stu-id="cbb18-163">This example uses *myOSDisk* for the disk name.</span></span>

<span data-ttu-id="cbb18-164">Vytvořte novou skupinu prostředků pro nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="cbb18-164">Create a new resource group for the new VM.</span></span>

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location -Name $destinationResourceGroup
```

<span data-ttu-id="cbb18-165">Nastavte název disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="cbb18-165">Set the OS disk name.</span></span> 

```powershell
$osDiskName = 'myOsDisk'
```

<span data-ttu-id="cbb18-166">Vytvoření spravovaného disku.</span><span class="sxs-lookup"><span data-stu-id="cbb18-166">Create the managed disk.</span></span>

```powershell
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig  -Location $location -CreateOption Copy `
    -SourceResourceId $snapshot.Id) `
    -ResourceGroupName $destinationResourceGroup
```


## <a name="create-the-new-vm"></a><span data-ttu-id="cbb18-167">Vytvoření nového virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cbb18-167">Create the new VM</span></span> 

<span data-ttu-id="cbb18-168">Vytvoření sítě a další prostředky virtuálních počítačů, které má být používána nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cbb18-168">Create networking and other VM resources to be used by the new VM.</span></span>

### <a name="create-the-subnet-and-vnet"></a><span data-ttu-id="cbb18-169">Vytvoření podsítě a sítě vNet</span><span class="sxs-lookup"><span data-stu-id="cbb18-169">Create the subNet and vNet</span></span>

<span data-ttu-id="cbb18-170">Vytvořit virtuální síť a podsíť [virtuální sítě](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="cbb18-170">Create the vNet and subNet of the [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

<span data-ttu-id="cbb18-171">Vytvořte podsíť.</span><span class="sxs-lookup"><span data-stu-id="cbb18-171">Create the subNet.</span></span> <span data-ttu-id="cbb18-172">Tento příklad vytvoří podsíť s názvem **mySubNet**, ve skupině prostředků **myDestinationResourceGroup**a nastaví předpona adresy podsítě **10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="cbb18-172">This example creates a subnet named **mySubNet**, in the resource group **myDestinationResourceGroup**, and sets the subnet address prefix to **10.0.0.0/24**.</span></span>
   
```powershell
$subnetName = 'mySubNet'
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
```

<span data-ttu-id="cbb18-173">Vytvořte síť vNet.</span><span class="sxs-lookup"><span data-stu-id="cbb18-173">Create the vNet.</span></span> <span data-ttu-id="cbb18-174">Tento příklad nastaví název virtuální sítě, který se má **myVnetName**, umístění, do **západní USA**a předponu adresy virtuální sítě, abyste **10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="cbb18-174">This example sets the virtual network name to be **myVnetName**, the location to **West US**, and the address prefix for the virtual network to **10.0.0.0/16**.</span></span> 
   
```powershell
$vnetName = "myVnetName"
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $destinationResourceGroup -Location $location `
    -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
```    


### <a name="create-the-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="cbb18-175">Vytvořte skupinu zabezpečení sítě a pravidlo protokolu RDP</span><span class="sxs-lookup"><span data-stu-id="cbb18-175">Create the network security group and an RDP rule</span></span>
<span data-ttu-id="cbb18-176">Abyste mohli přihlásit k virtuálnímu počítači pomocí protokolu RDP, musíte mít pravidlo zabezpečení, která umožňuje přístup RDP na portu 3389.</span><span class="sxs-lookup"><span data-stu-id="cbb18-176">To be able to log in to your VM using RDP, you need to have a security rule that allows RDP access on port 3389.</span></span> <span data-ttu-id="cbb18-177">Protože virtuálního pevného disku pro nový virtuální počítač byl vytvořen z existující speciální virtuální počítač, můžete použít účet ze zdrojového virtuálního počítače pro protokol RDP.</span><span class="sxs-lookup"><span data-stu-id="cbb18-177">Because the VHD for the new VM was created from an existing specialized VM, you can use an account from the source virtual machine for RDP.</span></span>

<span data-ttu-id="cbb18-178">Tento příklad nastaví název skupiny NSG na **myNsg** a název pravidla protokolu RDP na **myRdpRule**.</span><span class="sxs-lookup"><span data-stu-id="cbb18-178">This example sets the NSG name to **myNsg** and the RDP rule name to **myRdpRule**.</span></span>

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $destinationResourceGroup -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

<span data-ttu-id="cbb18-179">Další informace o koncových bodů a pravidla NSG najdete v tématu [otevřít porty pro virtuální počítač v Azure pomocí prostředí PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cbb18-179">For more information about endpoints and NSG rules, see [Opening ports to a VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

### <a name="create-a-public-ip-address-and-nic"></a><span data-ttu-id="cbb18-180">Vytvoření veřejné IP adresy a síťové karty</span><span class="sxs-lookup"><span data-stu-id="cbb18-180">Create a public IP address and NIC</span></span>
<span data-ttu-id="cbb18-181">Pokud chcete povolit komunikaci s virtuálním počítačem ve virtuální síti, budete potřebovat [veřejnou adresu IP](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) a síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cbb18-181">To enable communication with the virtual machine in the virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

<span data-ttu-id="cbb18-182">Vytvoření veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="cbb18-182">Create the public IP.</span></span> <span data-ttu-id="cbb18-183">V tomto příkladu je název veřejné IP adresy nastavena **myIP**.</span><span class="sxs-lookup"><span data-stu-id="cbb18-183">In this example, the public IP address name is set to **myIP**.</span></span>
   
```powershell
$ipName = "myIP"
$pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $destinationResourceGroup -Location $location `
   -AllocationMethod Dynamic
```       

<span data-ttu-id="cbb18-184">Vytvořit síťovou kartu.</span><span class="sxs-lookup"><span data-stu-id="cbb18-184">Create the NIC.</span></span> <span data-ttu-id="cbb18-185">V tomto příkladu je síťový adaptér je nastavit název **myNicName**.</span><span class="sxs-lookup"><span data-stu-id="cbb18-185">In this example, the NIC name is set to **myNicName**.</span></span>
   
```powershell
$nicName = "myNicName"
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $destinationResourceGroup `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```



### <a name="set-the-vm-name-and-size"></a><span data-ttu-id="cbb18-186">Nastavte název virtuálního počítače a velikosti</span><span class="sxs-lookup"><span data-stu-id="cbb18-186">Set the VM name and size</span></span>

<span data-ttu-id="cbb18-187">Tento příklad nastaví název virtuálního počítače na *Můjvp* a velikost virtuálního počítače na *Standard_A2*.</span><span class="sxs-lookup"><span data-stu-id="cbb18-187">This example sets the VM name to *myVM* and the VM size to *Standard_A2*.</span></span>

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-the-nic"></a><span data-ttu-id="cbb18-188">Přidání síťového adaptéru</span><span class="sxs-lookup"><span data-stu-id="cbb18-188">Add the NIC</span></span>
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    

### <a name="add-the-os-disk"></a><span data-ttu-id="cbb18-189">Přidat disk operačního systému</span><span class="sxs-lookup"><span data-stu-id="cbb18-189">Add the OS disk</span></span> 

<span data-ttu-id="cbb18-190">Přidat disk operačního systému do konfigurace pomocí [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk).</span><span class="sxs-lookup"><span data-stu-id="cbb18-190">Add the OS disk to the configuration using [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk).</span></span> <span data-ttu-id="cbb18-191">Tento příklad nastaví velikost disku k *128 GB* a připojí spravovaných disků jako *Windows* disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="cbb18-191">This example sets the size of the disk to *128 GB* and attaches the managed disk as a *Windows* OS disk.</span></span>
 
```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -StorageAccountType StandardLRS `
    -DiskSizeInGB 128 -CreateOption Attach -Windows
```

### <a name="complete-the-vm"></a><span data-ttu-id="cbb18-192">Dokončení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cbb18-192">Complete the VM</span></span> 

<span data-ttu-id="cbb18-193">Vytvořit virtuální počítač pomocí [New-AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm)konfigurace, které jsme právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="cbb18-193">Create the VM using [New-AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm)the configurations that we just created.</span></span>

```powershell
New-AzureRmVM -ResourceGroupName $destinationResourceGroup -Location $location -VM $vm
```

<span data-ttu-id="cbb18-194">Pokud tento příkaz byl úspěšný, zobrazí se výstup takto:</span><span class="sxs-lookup"><span data-stu-id="cbb18-194">If this command was successful, you'll see output like this:</span></span>

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-the-vm-was-created"></a><span data-ttu-id="cbb18-195">Zkontrolujte, zda byl vytvořen virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="cbb18-195">Verify that the VM was created</span></span>
<span data-ttu-id="cbb18-196">Měli byste vidět nově vytvořený virtuální počítač buď v [portál Azure](https://portal.azure.com)v části **Procházet** > **virtuální počítače**, nebo pomocí následujících příkazů prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="cbb18-196">You should see the newly created VM either in the [Azure portal](https://portal.azure.com), under **Browse** > **Virtual machines**, or by using the following PowerShell commands:</span></span>

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $destinationResourceGroup
$vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="cbb18-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cbb18-197">Next steps</span></span>
<span data-ttu-id="cbb18-198">K přihlášení do nového virtuálního počítače, přejděte do virtuálního počítače v [portál](https://portal.azure.com), klikněte na tlačítko **Connect**a otevřete soubor Remote Desktop RDP.</span><span class="sxs-lookup"><span data-stu-id="cbb18-198">To sign in to your new virtual machine, browse to the VM in the [portal](https://portal.azure.com), click **Connect**, and open the Remote Desktop RDP file.</span></span> <span data-ttu-id="cbb18-199">Pomocí přihlašovacích údajů účtu původní virtuálního počítače pro přihlášení do nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cbb18-199">Use the account credentials of your original virtual machine to sign in to your new virtual machine.</span></span> <span data-ttu-id="cbb18-200">Další informace najdete v tématu [jak se připojit a přihlásit se na virtuálním počítači Azure s Windows](connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="cbb18-200">For more information, see [How to connect and log on to an Azure virtual machine running Windows](connect-logon.md).</span></span>

