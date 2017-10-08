---
title: "aaaCreate virtuální počítač s Windows z specializované virtuálního pevného disku v Azure | Microsoft Docs"
description: "Vytvořte nový virtuální počítač Windows připojením specializované spravované disk jako disk s operačním systémem hello použití v modelu nasazení Resource Manager hello."
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
ms.openlocfilehash: c72c6f4fb650e2664e87cf38ec9be62f0b2209fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-vm-from-a-specialized-disk"></a><span data-ttu-id="23ae2-103">Vytvoření virtuálního počítače s Windows z specializované disku</span><span class="sxs-lookup"><span data-stu-id="23ae2-103">Create a Windows VM from a specialized disk</span></span>

<span data-ttu-id="23ae2-104">Vytvoření nového virtuálního počítače připojením specializované spravované disk jako disk s operačním systémem hello pomocí prostředí Powershell.</span><span class="sxs-lookup"><span data-stu-id="23ae2-104">Create a new VM by attaching a specialized managed disk as hello OS disk using Powershell.</span></span> <span data-ttu-id="23ae2-105">Specializované disk je kopie virtuálního pevného disku (VHD) ze stávajícího virtuálního počítače, který udržuje hello uživatelské účty, aplikace a další data o stavu z vašeho původního virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="23ae2-105">A specialized disk is a copy of virtual hard disk (VHD) from an existing VM that maintains hello user accounts, applications, and other state data from your original VM.</span></span> 

<span data-ttu-id="23ae2-106">Pokud používáte specializované toocreate virtuální pevný disk nového virtuálního počítače, hello nový virtuální počítač bude mít název počítače hello hello původní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="23ae2-106">When you use a specialized VHD toocreate a new VM, hello new VM retains hello computer name of hello original VM.</span></span> <span data-ttu-id="23ae2-107">Další informace specifické pro počítač se také nacházet a v některých případech tyto duplicitní informace může způsobit problémy.</span><span class="sxs-lookup"><span data-stu-id="23ae2-107">Other computer-specific information is also be kept and, in some cases, this duplicate information could cause issues.</span></span> <span data-ttu-id="23ae2-108">Být vědět, jaké typy informace specifické pro počítač aplikace se spoléhají na při kopírování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="23ae2-108">Be aware of what types of computer-specific information your applications rely on when copying a VM.</span></span>

<span data-ttu-id="23ae2-109">Máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="23ae2-109">You have two options:</span></span>
* [<span data-ttu-id="23ae2-110">Nahrání virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="23ae2-110">Upload a VHD</span></span>](#option-1-upload-a-specialized-vhd)
* [<span data-ttu-id="23ae2-111">Zkopírujte existující virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="23ae2-111">Copy an existing Azure VM</span></span>](#option-2-copy-an-existing-azure-vm)

<span data-ttu-id="23ae2-112">Toto téma ukazuje, jak spravovat toouse disky.</span><span class="sxs-lookup"><span data-stu-id="23ae2-112">This topic shows you how toouse managed disks.</span></span> <span data-ttu-id="23ae2-113">Pokud máte starší verzi nasazení, vyžaduje použití účtu úložiště najdete v tématu [vytvoření virtuálního počítače z specializované virtuálního pevného disku v účtu úložiště](sa-create-vm-specialized.md)</span><span class="sxs-lookup"><span data-stu-id="23ae2-113">If you have a legacy deployment that requires using a storage account, see [Create a VM from a specialized VHD in a storage account](sa-create-vm-specialized.md)</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="23ae2-114">Než začnete</span><span class="sxs-lookup"><span data-stu-id="23ae2-114">Before you begin</span></span>
<span data-ttu-id="23ae2-115">Pokud používáte prostředí PowerShell, ujistěte se, že máte nejnovější verzi hello modulu AzureRM.Compute PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="23ae2-115">If you use PowerShell, make sure that you have hello latest version of hello AzureRM.Compute PowerShell module.</span></span> 

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="23ae2-116">Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="23ae2-116">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="option-1-upload-a-specialized-vhd"></a><span data-ttu-id="23ae2-117">Možnost 1: Nahrát specializované virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="23ae2-117">Option 1: Upload a specialized VHD</span></span>

<span data-ttu-id="23ae2-118">Můžete nahrát hello virtuální pevný disk z virtuálního počítače se specializovanou vytvořené nástrojem místní virtualizace, jako je technologie Hyper-V nebo na virtuální počítač exportovat z jiného cloudu.</span><span class="sxs-lookup"><span data-stu-id="23ae2-118">You can upload hello VHD from a specialized VM created with an on-premises virtualization tool, like Hyper-V, or a VM exported from another cloud.</span></span>

### <a name="prepare-hello-vm"></a><span data-ttu-id="23ae2-119">Příprava hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="23ae2-119">Prepare hello VM</span></span>
<span data-ttu-id="23ae2-120">Pokud máte v úmyslu toouse hello virtuálního pevného disku jako-je toocreate nový virtuální počítač, je doplnit hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="23ae2-120">If you intend toouse hello VHD as-is toocreate a new VM, ensure hello following steps are completed.</span></span> 
  
  * <span data-ttu-id="23ae2-121">[Příprava virtuální pevný disk Windows tooupload tooAzure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="23ae2-121">[Prepare a Windows VHD tooupload tooAzure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="23ae2-122">**Nechcete** generalize hello virtuálního počítače pomocí nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="23ae2-122">**Do not** generalize hello VM using Sysprep.</span></span>
  * <span data-ttu-id="23ae2-123">Odeberte všechny hosta virtualizačních nástrojů a agentů, které jsou nainstalované na hello virtuálního počítače (například nástroje VMware).</span><span class="sxs-lookup"><span data-stu-id="23ae2-123">Remove any guest virtualization tools and agents that are installed on hello VM (like VMware tools).</span></span>
  * <span data-ttu-id="23ae2-124">Ujistěte se, hello virtuální počítač je nakonfigurovaný toopull jeho IP adresu a nastavení DNS pomocí protokolu DHCP.</span><span class="sxs-lookup"><span data-stu-id="23ae2-124">Ensure hello VM is configured toopull its IP address and DNS settings via DHCP.</span></span> <span data-ttu-id="23ae2-125">Tím se zajistí, že tento server hello získá IP adresu v rámci hello virtuální síť při spuštění.</span><span class="sxs-lookup"><span data-stu-id="23ae2-125">This ensures that hello server obtains an IP address within hello VNet when it starts up.</span></span> 


### <a name="get-hello-storage-account"></a><span data-ttu-id="23ae2-126">Získat účet úložiště hello</span><span class="sxs-lookup"><span data-stu-id="23ae2-126">Get hello storage account</span></span>
<span data-ttu-id="23ae2-127">Je třeba úložiště účtu v Azure toostore hello nahrát virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="23ae2-127">You need a storage account in Azure toostore hello uploaded VHD.</span></span> <span data-ttu-id="23ae2-128">Můžete použít existující účet úložiště, nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="23ae2-128">You can either use an existing storage account or create a new one.</span></span> 

<span data-ttu-id="23ae2-129">účty úložiště k dispozici hello tooshow, zadejte:</span><span class="sxs-lookup"><span data-stu-id="23ae2-129">tooshow hello available storage accounts, type:</span></span>

```powershell
Get-AzureRmStorageAccount
```

<span data-ttu-id="23ae2-130">Pokud chcete toouse stávající účet úložiště, pokračovat toohello [hello nahrání virtuálního pevného disku](#upload-the-vhd-to-your-storage-account) části.</span><span class="sxs-lookup"><span data-stu-id="23ae2-130">If you want toouse an existing storage account, proceed toohello [Upload hello VHD](#upload-the-vhd-to-your-storage-account) section.</span></span>

<span data-ttu-id="23ae2-131">Pokud potřebujete toocreate účet úložiště, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="23ae2-131">If you need toocreate a storage account, follow these steps:</span></span>

1. <span data-ttu-id="23ae2-132">Je nutné hello název skupiny prostředků hello, kde se vytvořit účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="23ae2-132">You need hello name of hello resource group where hello storage account should be created.</span></span> <span data-ttu-id="23ae2-133">toofind se všechny skupiny zdrojů hello, které jsou v rámci vašeho předplatného, typ:</span><span class="sxs-lookup"><span data-stu-id="23ae2-133">toofind out all hello resource groups that are in your subscription, type:</span></span>
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    <span data-ttu-id="23ae2-134">skupinu prostředků s názvem toocreate *myResourceGroup* v hello *západní USA* oblast, zadejte:</span><span class="sxs-lookup"><span data-stu-id="23ae2-134">toocreate a resource group named *myResourceGroup* in hello *West US* region, type:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. <span data-ttu-id="23ae2-135">Vytvořit účet úložiště s názvem *můj_účet_úložiště* v této skupině prostředků s použitím hello [AzureRmStorageAccount nový](/powershell/module/azurerm.storage/new-azurermstorageaccount) rutiny:</span><span class="sxs-lookup"><span data-stu-id="23ae2-135">Create a storage account named *mystorageaccount* in this resource group by using hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span></span>
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```

### <a name="upload-hello-vhd-tooyour-storage-account"></a><span data-ttu-id="23ae2-136">Nahrát účet úložiště tooyour hello virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="23ae2-136">Upload hello VHD tooyour storage account</span></span> 
<span data-ttu-id="23ae2-137">Použití hello [přidat AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) rutiny tooupload hello virtuálního pevného disku tooa kontejneru v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="23ae2-137">Use hello [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet tooupload hello VHD tooa container in your storage account.</span></span> <span data-ttu-id="23ae2-138">Tento příklad nahrávání hello souboru *myVHD.vhd* z `"C:\Users\Public\Documents\Virtual hard disks\"` tooa účet úložiště s názvem *můj_účet_úložiště* v hello *myResourceGroup* skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="23ae2-138">This example uploads hello file *myVHD.vhd* from `"C:\Users\Public\Documents\Virtual hard disks\"` tooa storage account named *mystorageaccount* in hello *myResourceGroup* resource group.</span></span> <span data-ttu-id="23ae2-139">Hello soubor je uložen v hello kontejner s názvem *můj_kontejner* a bude nový název souboru hello *myUploadedVHD.vhd*.</span><span class="sxs-lookup"><span data-stu-id="23ae2-139">hello file is stored in hello container named *mycontainer* and hello new file name will be *myUploadedVHD.vhd*.</span></span>

```powershell
$resourceGroupName = "myResourceGroup"
$urlOfUploadedVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $resourceGroupName -Destination $urlOfUploadedVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


<span data-ttu-id="23ae2-140">Pokud bylo úspěšné, můžete získat odpovědi, která vypadá podobně jako toothis:</span><span class="sxs-lookup"><span data-stu-id="23ae2-140">If successful, you get a response that looks similar toothis:</span></span>

```powershell
MD5 hash is being calculated for hello file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for hello operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

<span data-ttu-id="23ae2-141">V závislosti na vašem síťovém připojení a hello velikost vašeho souboru virtuálního pevného disku, tento příkaz může chvíli trvat toocomplete</span><span class="sxs-lookup"><span data-stu-id="23ae2-141">Depending on your network connection and hello size of your VHD file, this command may take a while toocomplete</span></span>

### <a name="create-a-managed-disk-from-hello-vhd"></a><span data-ttu-id="23ae2-142">Vytvoření spravovaného disku ze hello virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="23ae2-142">Create a managed disk from hello VHD</span></span>

<span data-ttu-id="23ae2-143">Vytvoření spravovaného disku ze hello specializuje virtuálního pevného disku v účtu úložiště pomocí [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk).</span><span class="sxs-lookup"><span data-stu-id="23ae2-143">Create a managed disk from hello specialized VHD in your storage account using [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk).</span></span> <span data-ttu-id="23ae2-144">Tento příklad používá **myOSDisk1** pro název disku hello, PUT hello disk v *StandardLRS* úložiště a používá *https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd* jako hello identifikátor URI pro hello zdrojového virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="23ae2-144">This example uses **myOSDisk1** for hello disk name, puts hello disk in *StandardLRS* storage, and uses *https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd* as hello URI for hello source VHD.</span></span>

<span data-ttu-id="23ae2-145">Vytvořit novou skupinu prostředků pro hello nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="23ae2-145">Create a new resource group for hello new VM.</span></span>

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location -Name $destinationResourceGroup
```

<span data-ttu-id="23ae2-146">Vytvořit nový disk operačního systému hello z hello nahrát virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="23ae2-146">Create hello new OS disk from hello uploaded VHD.</span></span> 

```powershell
$sourceUri = https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd)
$osDiskName = 'myOsDisk'
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig -AccountType StandardLRS  -Location $location -CreateOption Import `
    -SourceUri $sourceUri) `
    -ResourceGroupName $destinationResourceGroup
```

## <a name="option-2-copy-an-existing-azure-vm"></a><span data-ttu-id="23ae2-147">Možnost 2: Zkopírujte existující virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="23ae2-147">Option 2: Copy an existing Azure VM</span></span>

<span data-ttu-id="23ae2-148">Můžete vytvořit kopii virtuálního počítače, že používá spravuje disky pořízení snímku hello virtuálních počítačů, a potom pomocí této toocreate snímku nové spravovaného disku a vytvoření nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="23ae2-148">You can create a copy of a VM that uses managed disks by taking a snapshot of hello VM, then using that snapshot toocreate a new managed disk and a new VM.</span></span>


### <a name="take-a-snapshot-of-hello-os-disk"></a><span data-ttu-id="23ae2-149">Pořízení snímku hello disk operačního systému</span><span class="sxs-lookup"><span data-stu-id="23ae2-149">Take a snapshot of hello OS disk</span></span>

<span data-ttu-id="23ae2-150">Můžete provést z snímek a celý virtuální počítač (včetně všech disků) nebo právě jeden disk.</span><span class="sxs-lookup"><span data-stu-id="23ae2-150">You can take a snapshot of and entire VM (including all disks) or of just a single disk.</span></span> <span data-ttu-id="23ae2-151">Hello následující kroky vám ukážou, jak tootake snímek právě hello operačního systému disku virtuální počítač pomocí hello [New-AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) rutiny.</span><span class="sxs-lookup"><span data-stu-id="23ae2-151">hello following steps show you how tootake a snapshot of just hello OS disk of your VM using hello [New-AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) cmdlet.</span></span> 

<span data-ttu-id="23ae2-152">Nastavte některé parametry.</span><span class="sxs-lookup"><span data-stu-id="23ae2-152">Set some parameters.</span></span> 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$vmName = 'myVM'
$location = 'westus' 
$snapshotName = 'mySnapshot'  
```

<span data-ttu-id="23ae2-153">Získejte objekt virtuálního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="23ae2-153">Get hello VM object.</span></span>

```powershell
$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $resourceGroupName
```
<span data-ttu-id="23ae2-154">Získáte název disku hello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="23ae2-154">Get hello OS disk name.</span></span>

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $vm.StorageProfile.OsDisk.Name
```

<span data-ttu-id="23ae2-155">Vytvoření snímku konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="23ae2-155">Create hello snapshot configuration.</span></span> 

 ```powershell
$snapshotConfig =  New-AzureRmSnapshotConfig -SourceUri $disk.Id -OsType Windows -CreateOption Copy -Location $location 
```

<span data-ttu-id="23ae2-156">Pořízení snímku hello.</span><span class="sxs-lookup"><span data-stu-id="23ae2-156">Take hello snapshot.</span></span>

```powershell
$snapShot = New-AzureRmSnapshot -Snapshot $snapshotConfig -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName
```


<span data-ttu-id="23ae2-157">Pokud máte v plánu toouse hello snímku toocreate virtuální počítač, který potřebuje toobe vysoké provádění, použijte parametr hello `-AccountType Premium_LRS` pomocí příkazu New-AzureRmSnapshot hello.</span><span class="sxs-lookup"><span data-stu-id="23ae2-157">If you plan toouse hello snapshot toocreate a VM that needs toobe high performing, use hello parameter `-AccountType Premium_LRS` with hello New-AzureRmSnapshot command.</span></span> <span data-ttu-id="23ae2-158">Parametr Hello vytvoří snímek hello tak, aby je uložena jako spravovaný Disk úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="23ae2-158">hello parameter creates hello snapshot so that it's stored as a Premium Managed Disk.</span></span> <span data-ttu-id="23ae2-159">Premium spravované disky jsou dražší než Standard.</span><span class="sxs-lookup"><span data-stu-id="23ae2-159">Premium Managed Disks are more expensive than Standard.</span></span> <span data-ttu-id="23ae2-160">Proto ujistěte se, že je skutečně potřebujete Premium před použitím parametru hello.</span><span class="sxs-lookup"><span data-stu-id="23ae2-160">So be sure you really need Premium before using hello parameter.</span></span>

### <a name="create-a-new-disk-from-hello-snapshot"></a><span data-ttu-id="23ae2-161">Vytvoření nového disku ze snímku hello</span><span class="sxs-lookup"><span data-stu-id="23ae2-161">Create a new disk from hello snapshot</span></span>

<span data-ttu-id="23ae2-162">Vytvoření spravovaného disku ze snímku pomocí hello [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk).</span><span class="sxs-lookup"><span data-stu-id="23ae2-162">Create a managed disk from hello snapshot using [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk).</span></span> <span data-ttu-id="23ae2-163">Tento příklad používá *myOSDisk* pro název disku hello.</span><span class="sxs-lookup"><span data-stu-id="23ae2-163">This example uses *myOSDisk* for hello disk name.</span></span>

<span data-ttu-id="23ae2-164">Vytvořit novou skupinu prostředků pro hello nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="23ae2-164">Create a new resource group for hello new VM.</span></span>

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location -Name $destinationResourceGroup
```

<span data-ttu-id="23ae2-165">Nastavte název disku hello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="23ae2-165">Set hello OS disk name.</span></span> 

```powershell
$osDiskName = 'myOsDisk'
```

<span data-ttu-id="23ae2-166">Vytvoření hello spravovaného disku.</span><span class="sxs-lookup"><span data-stu-id="23ae2-166">Create hello managed disk.</span></span>

```powershell
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig  -Location $location -CreateOption Copy `
    -SourceResourceId $snapshot.Id) `
    -ResourceGroupName $destinationResourceGroup
```


## <a name="create-hello-new-vm"></a><span data-ttu-id="23ae2-167">Vytvoření nového virtuálního počítače hello</span><span class="sxs-lookup"><span data-stu-id="23ae2-167">Create hello new VM</span></span> 

<span data-ttu-id="23ae2-168">Vytvoření sítě a jiných toobe prostředky virtuálních počítačů používané hello nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="23ae2-168">Create networking and other VM resources toobe used by hello new VM.</span></span>

### <a name="create-hello-subnet-and-vnet"></a><span data-ttu-id="23ae2-169">Vytvoření podsítě hello a virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="23ae2-169">Create hello subNet and vNet</span></span>

<span data-ttu-id="23ae2-170">Vytvoření hello virtuální síť a podsíť hello [virtuální sítě](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="23ae2-170">Create hello vNet and subNet of hello [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

<span data-ttu-id="23ae2-171">Vytvořte podsíť hello.</span><span class="sxs-lookup"><span data-stu-id="23ae2-171">Create hello subNet.</span></span> <span data-ttu-id="23ae2-172">Tento příklad vytvoří podsíť s názvem **mySubNet**, ve skupině prostředků hello **myDestinationResourceGroup**, a nastaví hello předpona adresy podsítě příliš**10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="23ae2-172">This example creates a subnet named **mySubNet**, in hello resource group **myDestinationResourceGroup**, and sets hello subnet address prefix too**10.0.0.0/24**.</span></span>
   
```powershell
$subnetName = 'mySubNet'
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
```

<span data-ttu-id="23ae2-173">Vytvoření sítě vNet hello.</span><span class="sxs-lookup"><span data-stu-id="23ae2-173">Create hello vNet.</span></span> <span data-ttu-id="23ae2-174">Tento příklad sady hello toobe název virtuální sítě **myVnetName**, hello umístění příliš**západní USA**, a příliš hello předpona adresy pro virtuální síť hello**10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="23ae2-174">This example sets hello virtual network name toobe **myVnetName**, hello location too**West US**, and hello address prefix for hello virtual network too**10.0.0.0/16**.</span></span> 
   
```powershell
$vnetName = "myVnetName"
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $destinationResourceGroup -Location $location `
    -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
```    


### <a name="create-hello-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="23ae2-175">Vytvořte skupinu zabezpečení sítě hello a pravidlo protokolu RDP</span><span class="sxs-lookup"><span data-stu-id="23ae2-175">Create hello network security group and an RDP rule</span></span>
<span data-ttu-id="23ae2-176">možnost toolog toobe v tooyour virtuálního počítače pomocí protokolu RDP, je nutné toohave pravidlo zabezpečení, která umožňuje přístup RDP na portu 3389.</span><span class="sxs-lookup"><span data-stu-id="23ae2-176">toobe able toolog in tooyour VM using RDP, you need toohave a security rule that allows RDP access on port 3389.</span></span> <span data-ttu-id="23ae2-177">Protože hello virtuálního pevného disku pro nový virtuální počítač byl vytvořen z existující virtuální počítač specializované hello, můžete použít účet z hello zdrojového virtuálního počítače pro protokol RDP.</span><span class="sxs-lookup"><span data-stu-id="23ae2-177">Because hello VHD for hello new VM was created from an existing specialized VM, you can use an account from hello source virtual machine for RDP.</span></span>

<span data-ttu-id="23ae2-178">Tento příklad sady hello NSG název příliš**myNsg** a hello RDP název pravidla příliš**myRdpRule**.</span><span class="sxs-lookup"><span data-stu-id="23ae2-178">This example sets hello NSG name too**myNsg** and hello RDP rule name too**myRdpRule**.</span></span>

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $destinationResourceGroup -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

<span data-ttu-id="23ae2-179">Další informace o koncových bodů a pravidla NSG najdete v tématu [otevřít porty tooa virtuálních počítačů v Azure pomocí prostředí PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="23ae2-179">For more information about endpoints and NSG rules, see [Opening ports tooa VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

### <a name="create-a-public-ip-address-and-nic"></a><span data-ttu-id="23ae2-180">Vytvoření veřejné IP adresy a síťové karty</span><span class="sxs-lookup"><span data-stu-id="23ae2-180">Create a public IP address and NIC</span></span>
<span data-ttu-id="23ae2-181">tooenable komunikaci s hello virtuálním počítačem ve virtuální síti hello, musíte [veřejnou IP adresu](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) a síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="23ae2-181">tooenable communication with hello virtual machine in hello virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

<span data-ttu-id="23ae2-182">Vytvoření veřejné IP adresy hello.</span><span class="sxs-lookup"><span data-stu-id="23ae2-182">Create hello public IP.</span></span> <span data-ttu-id="23ae2-183">V tomto příkladu je název hello veřejné IP adresy nastavit příliš**myIP**.</span><span class="sxs-lookup"><span data-stu-id="23ae2-183">In this example, hello public IP address name is set too**myIP**.</span></span>
   
```powershell
$ipName = "myIP"
$pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $destinationResourceGroup -Location $location `
   -AllocationMethod Dynamic
```       

<span data-ttu-id="23ae2-184">Vytvoření hello síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="23ae2-184">Create hello NIC.</span></span> <span data-ttu-id="23ae2-185">V tomto příkladu název hello síťový adaptér se nastaví příliš**myNicName**.</span><span class="sxs-lookup"><span data-stu-id="23ae2-185">In this example, hello NIC name is set too**myNicName**.</span></span>
   
```powershell
$nicName = "myNicName"
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $destinationResourceGroup `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```



### <a name="set-hello-vm-name-and-size"></a><span data-ttu-id="23ae2-186">Nastavte název virtuálního počítače hello a velikosti</span><span class="sxs-lookup"><span data-stu-id="23ae2-186">Set hello VM name and size</span></span>

<span data-ttu-id="23ae2-187">Tento příklad sady hello název virtuálního počítače příliš*Můjvp* a velikost hello virtuálního počítače příliš*Standard_A2*.</span><span class="sxs-lookup"><span data-stu-id="23ae2-187">This example sets hello VM name too*myVM* and hello VM size too*Standard_A2*.</span></span>

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-hello-nic"></a><span data-ttu-id="23ae2-188">Přidat hello síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="23ae2-188">Add hello NIC</span></span>
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    

### <a name="add-hello-os-disk"></a><span data-ttu-id="23ae2-189">Přidání disku hello operačního systému</span><span class="sxs-lookup"><span data-stu-id="23ae2-189">Add hello OS disk</span></span> 

<span data-ttu-id="23ae2-190">Přidat hello operačního systému disku toohello konfigurace pomocí [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk).</span><span class="sxs-lookup"><span data-stu-id="23ae2-190">Add hello OS disk toohello configuration using [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk).</span></span> <span data-ttu-id="23ae2-191">Tento příklad nastaví hello velikost disku hello příliš*128 GB* a připojí hello spravovaných disků jako *Windows* disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="23ae2-191">This example sets hello size of hello disk too*128 GB* and attaches hello managed disk as a *Windows* OS disk.</span></span>
 
```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -StorageAccountType StandardLRS `
    -DiskSizeInGB 128 -CreateOption Attach -Windows
```

### <a name="complete-hello-vm"></a><span data-ttu-id="23ae2-192">Dokončení hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="23ae2-192">Complete hello VM</span></span> 

<span data-ttu-id="23ae2-193">Vytvořit virtuální počítač pomocí hello [New-AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm)hello konfigurace, které jsme právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="23ae2-193">Create hello VM using [New-AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm)hello configurations that we just created.</span></span>

```powershell
New-AzureRmVM -ResourceGroupName $destinationResourceGroup -Location $location -VM $vm
```

<span data-ttu-id="23ae2-194">Pokud tento příkaz byl úspěšný, zobrazí se výstup takto:</span><span class="sxs-lookup"><span data-stu-id="23ae2-194">If this command was successful, you'll see output like this:</span></span>

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-hello-vm-was-created"></a><span data-ttu-id="23ae2-195">Ověřte, že hello, ke které byl virtuální počítač vytvořen</span><span class="sxs-lookup"><span data-stu-id="23ae2-195">Verify that hello VM was created</span></span>
<span data-ttu-id="23ae2-196">Měli byste vidět hello nově vytvořený virtuální počítač buď v hello [portál Azure](https://portal.azure.com)v části **Procházet** > **virtuální počítače**, nebo pomocí prostředí PowerShell následující hello příkazy:</span><span class="sxs-lookup"><span data-stu-id="23ae2-196">You should see hello newly created VM either in hello [Azure portal](https://portal.azure.com), under **Browse** > **Virtual machines**, or by using hello following PowerShell commands:</span></span>

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $destinationResourceGroup
$vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="23ae2-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="23ae2-197">Next steps</span></span>
<span data-ttu-id="23ae2-198">toosign v tooyour nový virtuální počítač, procházet toohello virtuálních počítačů v hello [portál](https://portal.azure.com), klikněte na tlačítko **Connect**a soubor Remote Desktop RDP otevřete hello.</span><span class="sxs-lookup"><span data-stu-id="23ae2-198">toosign in tooyour new virtual machine, browse toohello VM in hello [portal](https://portal.azure.com), click **Connect**, and open hello Remote Desktop RDP file.</span></span> <span data-ttu-id="23ae2-199">Pomocí přihlašovacích údajů účtu hello z vašeho původního toosign virtuálního počítače v tooyour nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="23ae2-199">Use hello account credentials of your original virtual machine toosign in tooyour new virtual machine.</span></span> <span data-ttu-id="23ae2-200">Další informace najdete v tématu [jak se tooconnect a protokol na tooan Azure virtuální počítač, systém Windows spuštěný](connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="23ae2-200">For more information, see [How tooconnect and log on tooan Azure virtual machine running Windows](connect-logon.md).</span></span>

