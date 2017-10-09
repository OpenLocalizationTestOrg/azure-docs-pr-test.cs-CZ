---
title: "aaaCreate virtuálních počítačů z specializované disku v Azure | Microsoft Docs"
description: "Vytvoření nového virtuálního počítače připojením specializované nespravované disku, v modelu nasazení Resource Manager hello."
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
ms.date: 05/23/2017
ms.author: cynthn
ms.openlocfilehash: c88f213b6629a6c1d6ff5845e76c2f7719672714
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-from-a-specialized-vhd-in-a-storage-account"></a><span data-ttu-id="59e96-103">Vytvoření virtuálního počítače z specializované virtuálního pevného disku v účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="59e96-103">Create a VM from a specialized VHD in a storage account</span></span>

<span data-ttu-id="59e96-104">Vytvoření nového virtuálního počítače připojením specializované nespravované disk jako disk s operačním systémem hello pomocí prostředí Powershell.</span><span class="sxs-lookup"><span data-stu-id="59e96-104">Create a new VM by attaching a specialized unmanaged disk as hello OS disk using Powershell.</span></span> <span data-ttu-id="59e96-105">Specializované disk je kopie virtuálního pevného disku z existující virtuální počítač, který udržuje hello uživatelské účty, aplikace a další data o stavu z vašeho původního virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="59e96-105">A specialized disk is a copy of VHD from an existing VM that maintains hello user accounts, applications and other state data from your original VM.</span></span> 

<span data-ttu-id="59e96-106">Máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="59e96-106">You have two options:</span></span>
* [<span data-ttu-id="59e96-107">Nahrání virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="59e96-107">Upload a VHD</span></span>](create-vm-specialized.md#option-1-upload-a-specialized-vhd)
* [<span data-ttu-id="59e96-108">Zkopírujte hello virtuální pevný disk existující virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="59e96-108">Copy hello VHD of an existing Azure VM</span></span>](create-vm-specialized.md#option-2-copy-an-existing-azure-vm)

## <a name="before-you-begin"></a><span data-ttu-id="59e96-109">Než začnete</span><span class="sxs-lookup"><span data-stu-id="59e96-109">Before you begin</span></span>
<span data-ttu-id="59e96-110">Pokud používáte prostředí PowerShell, ujistěte se, že máte nejnovější verzi hello modulu AzureRM.Compute PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="59e96-110">If you use PowerShell, make sure that you have hello latest version of hello AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="59e96-111">Spustit hello následující příkaz tooinstall.</span><span class="sxs-lookup"><span data-stu-id="59e96-111">Run hello following command tooinstall it.</span></span>

```powershell
Install-Module AzureRM.Compute 
```
<span data-ttu-id="59e96-112">Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="59e96-112">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="option-1-upload-a-specialized-vhd"></a><span data-ttu-id="59e96-113">Možnost 1: Nahrát specializované virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="59e96-113">Option 1: Upload a specialized VHD</span></span>

<span data-ttu-id="59e96-114">Můžete nahrát hello virtuální pevný disk z virtuálního počítače se specializovanou vytvořené nástrojem místní virtualizace, jako je technologie Hyper-V nebo na virtuální počítač exportovat z jiného cloudu.</span><span class="sxs-lookup"><span data-stu-id="59e96-114">You can upload hello VHD from a specialized VM created with an on-premises virtualization tool, like Hyper-V, or a VM exported from another cloud.</span></span>

### <a name="prepare-hello-vm"></a><span data-ttu-id="59e96-115">Příprava hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="59e96-115">Prepare hello VM</span></span>
<span data-ttu-id="59e96-116">Můžete nahrát specializované VHD, který byl vytvořen pomocí místní počítač nebo virtuální pevný disk exportovaný z jiného cloudu.</span><span class="sxs-lookup"><span data-stu-id="59e96-116">You can upload a specialized VHD that was created using an on-premises VM or a VHD exported from another cloud.</span></span> <span data-ttu-id="59e96-117">Specializované virtuálního pevného disku udržuje hello uživatelské účty, aplikace a další data o stavu z vašeho původního virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="59e96-117">A specialized VHD maintains hello user accounts, applications and other state data from your original VM.</span></span> <span data-ttu-id="59e96-118">Pokud máte v úmyslu toouse hello virtuálního pevného disku jako-je toocreate nový virtuální počítač, je doplnit hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="59e96-118">If you intend toouse hello VHD as-is toocreate a new VM, ensure hello following steps are completed.</span></span> 
  
  * <span data-ttu-id="59e96-119">[Příprava virtuální pevný disk Windows tooupload tooAzure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="59e96-119">[Prepare a Windows VHD tooupload tooAzure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="59e96-120">**Nechcete** generalize hello virtuálního počítače pomocí nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="59e96-120">**Do not** generalize hello VM using Sysprep.</span></span>
  * <span data-ttu-id="59e96-121">Odeberte všechny hosta virtualizačních nástrojů a agentů, které jsou nainstalované na hello virtuálního počítače (tj. nástroje VMware).</span><span class="sxs-lookup"><span data-stu-id="59e96-121">Remove any guest virtualization tools and agents that are installed on hello VM (i.e. VMware tools).</span></span>
  * <span data-ttu-id="59e96-122">Ujistěte se, hello virtuální počítač je nakonfigurovaný toopull jeho IP adresu a nastavení DNS pomocí protokolu DHCP.</span><span class="sxs-lookup"><span data-stu-id="59e96-122">Ensure hello VM is configured toopull its IP address and DNS settings via DHCP.</span></span> <span data-ttu-id="59e96-123">Tím se zajistí, že tento server hello získá IP adresu v rámci hello virtuální síť při spuštění.</span><span class="sxs-lookup"><span data-stu-id="59e96-123">This ensures that hello server obtains an IP address within hello VNet when it starts up.</span></span> 


### <a name="get-hello-storage-account"></a><span data-ttu-id="59e96-124">Získat účet úložiště hello</span><span class="sxs-lookup"><span data-stu-id="59e96-124">Get hello storage account</span></span>
<span data-ttu-id="59e96-125">Je nutné účet úložiště na image virtuálního počítače Azure toostore hello nahrát.</span><span class="sxs-lookup"><span data-stu-id="59e96-125">You need a storage account in Azure toostore hello uploaded VM image.</span></span> <span data-ttu-id="59e96-126">Můžete použít existující účet úložiště, nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="59e96-126">You can either use an existing storage account or create a new one.</span></span> 

<span data-ttu-id="59e96-127">účty úložiště k dispozici hello tooshow, zadejte:</span><span class="sxs-lookup"><span data-stu-id="59e96-127">tooshow hello available storage accounts, type:</span></span>

```powershell
Get-AzureRmStorageAccount
```

<span data-ttu-id="59e96-128">Pokud chcete toouse stávající účet úložiště, pokračovat toohello [image virtuálního počítače hello nahrávání](#upload-the-vm-vhd-to-your-storage-account) části.</span><span class="sxs-lookup"><span data-stu-id="59e96-128">If you want toouse an existing storage account, proceed toohello [Upload hello VM image](#upload-the-vm-vhd-to-your-storage-account) section.</span></span>

<span data-ttu-id="59e96-129">Pokud potřebujete toocreate účet úložiště, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="59e96-129">If you need toocreate a storage account, follow these steps:</span></span>

1. <span data-ttu-id="59e96-130">Je nutné hello název skupiny prostředků hello, kde se vytvořit účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="59e96-130">You need hello name of hello resource group where hello storage account should be created.</span></span> <span data-ttu-id="59e96-131">toofind se všechny skupiny zdrojů hello, které jsou v rámci vašeho předplatného, typ:</span><span class="sxs-lookup"><span data-stu-id="59e96-131">toofind out all hello resource groups that are in your subscription, type:</span></span>
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    <span data-ttu-id="59e96-132">skupinu prostředků s názvem toocreate **myResourceGroup** v hello **západní USA** oblast, zadejte:</span><span class="sxs-lookup"><span data-stu-id="59e96-132">toocreate a resource group named **myResourceGroup** in hello **West US** region, type:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. <span data-ttu-id="59e96-133">Vytvořit účet úložiště s názvem **můj_účet_úložiště** v této skupině prostředků s použitím hello [AzureRmStorageAccount nový](/powershell/module/azurerm.storage/new-azurermstorageaccount) rutiny:</span><span class="sxs-lookup"><span data-stu-id="59e96-133">Create a storage account named **mystorageaccount** in this resource group by using hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span></span>
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
   
### <a name="upload-hello-vhd-tooyour-storage-account"></a><span data-ttu-id="59e96-134">Nahrát účet úložiště tooyour hello virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="59e96-134">Upload hello VHD tooyour storage account</span></span>
<span data-ttu-id="59e96-135">Použití hello [přidat AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) rutiny tooupload hello image tooa kontejneru v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="59e96-135">Use hello [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet tooupload hello image tooa container in your storage account.</span></span> <span data-ttu-id="59e96-136">Tento příklad nahrávání hello souboru **myVHD.vhd** z `"C:\Users\Public\Documents\Virtual hard disks\"` tooa účet úložiště s názvem **můj_účet_úložiště** v hello **myResourceGroup** skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="59e96-136">This example uploads hello file **myVHD.vhd** from `"C:\Users\Public\Documents\Virtual hard disks\"` tooa storage account named **mystorageaccount** in hello **myResourceGroup** resource group.</span></span> <span data-ttu-id="59e96-137">soubor Hello budou umístěny do hello kontejner s názvem **můj_kontejner** a bude nový název souboru hello **myUploadedVHD.vhd**.</span><span class="sxs-lookup"><span data-stu-id="59e96-137">hello file will be placed into hello container named **mycontainer** and hello new file name will be **myUploadedVHD.vhd**.</span></span>

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


<span data-ttu-id="59e96-138">Pokud bylo úspěšné, můžete získat odpovědi, která vypadá podobně jako toothis:</span><span class="sxs-lookup"><span data-stu-id="59e96-138">If successful, you get a response that looks similar toothis:</span></span>

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

<span data-ttu-id="59e96-139">V závislosti na vašem síťovém připojení a hello velikost vašeho souboru virtuálního pevného disku, tento příkaz může chvíli trvat toocomplete.</span><span class="sxs-lookup"><span data-stu-id="59e96-139">Depending on your network connection and hello size of your VHD file, this command may take a while toocomplete.</span></span>


## <a name="option-2-copy-hello-vhd-from-an-existing-azure-vm"></a><span data-ttu-id="59e96-140">Možnost 2: Kopírování hello virtuálního pevného disku z existující virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="59e96-140">Option 2: Copy hello VHD from an existing Azure VM</span></span>

<span data-ttu-id="59e96-141">Při vytváření nového virtuálního počítače duplicitní můžete zkopírovat toouse účet úložiště tooanother virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="59e96-141">You can copy a VHD tooanother storage account toouse when creating a new, duplicate VM.</span></span>

### <a name="before-you-begin"></a><span data-ttu-id="59e96-142">Než začnete</span><span class="sxs-lookup"><span data-stu-id="59e96-142">Before you begin</span></span>
<span data-ttu-id="59e96-143">Ujistěte se, že jste:</span><span class="sxs-lookup"><span data-stu-id="59e96-143">Make sure that you:</span></span>

* <span data-ttu-id="59e96-144">Neobsahuje informace o hello **zdrojové a cílové účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="59e96-144">Have information about hello **source and destination storage accounts**.</span></span> <span data-ttu-id="59e96-145">Pro hello zdrojového virtuálního počítače budete potřebovat názvy toohave hello úložiště účtu a kontejneru.</span><span class="sxs-lookup"><span data-stu-id="59e96-145">For hello source VM, you need toohave hello storage account and container names.</span></span> <span data-ttu-id="59e96-146">Obvykle bude název kontejneru hello **virtuální pevné disky**.</span><span class="sxs-lookup"><span data-stu-id="59e96-146">Usually, hello container name will be **vhds**.</span></span> <span data-ttu-id="59e96-147">Budete také potřebovat toohave cílový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="59e96-147">You also need toohave a destination storage account.</span></span> <span data-ttu-id="59e96-148">Pokud jste již nemáte, můžete vytvořit pomocí portálu buď hello (**více služeb** > účty úložiště > Přidat) nebo pomocí hello [AzureRmStorageAccount nový](/powershell/module/azurerm.storage/new-azurermstorageaccount) rutiny.</span><span class="sxs-lookup"><span data-stu-id="59e96-148">If you don't already have one, you can create one using either hello portal (**More Services** > Storage accounts > Add) or using hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet.</span></span> 
* <span data-ttu-id="59e96-149">Stáhli a nainstalovali hello [nástroj AzCopy](../../storage/common/storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="59e96-149">Have downloaded and installed hello [AzCopy tool](../../storage/common/storage-use-azcopy.md).</span></span> 

### <a name="deallocate-hello-vm"></a><span data-ttu-id="59e96-150">Deallocate hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="59e96-150">Deallocate hello VM</span></span>
<span data-ttu-id="59e96-151">Deallocate hello virtuálních počítačů, což uvolní toobe hello virtuální pevný disk zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="59e96-151">Deallocate hello VM, which frees up hello VHD toobe copied.</span></span> 

* <span data-ttu-id="59e96-152">**Portál**: klikněte na tlačítko **virtuální počítače** > **Můjvp** > Zastavit</span><span class="sxs-lookup"><span data-stu-id="59e96-152">**Portal**: Click **Virtual machines** > **myVM** > Stop</span></span>
* <span data-ttu-id="59e96-153">**Prostředí PowerShell**: použití [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) toostop (zrušit přidělení) hello virtuálního počítače s názvem **Můjvp** ve skupině prostředků **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="59e96-153">**Powershell**: Use [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) toostop (deallocate) hello VM named **myVM** in resource group **myResourceGroup**.</span></span>

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
```

<span data-ttu-id="59e96-154">Hello **stav** pro hello virtuálních počítačů v hello Azure portal změní z **Zastaveno** příliš**zastaveném (nepřiřazeném)**.</span><span class="sxs-lookup"><span data-stu-id="59e96-154">hello **Status** for hello VM in hello Azure portal changes from **Stopped** too**Stopped (deallocated)**.</span></span>

### <a name="get-hello-storage-account-urls"></a><span data-ttu-id="59e96-155">Získání adres URL účtu úložiště hello</span><span class="sxs-lookup"><span data-stu-id="59e96-155">Get hello storage account URLs</span></span>
<span data-ttu-id="59e96-156">Je nutné adresy URL hello hello zdrojové a cílové úložiště účtů.</span><span class="sxs-lookup"><span data-stu-id="59e96-156">You need hello URLs of hello source and destination storage accounts.</span></span> <span data-ttu-id="59e96-157">Hello adresy URL vypadat: `https://<storageaccount>.blob.core.windows.net/<containerName>/`.</span><span class="sxs-lookup"><span data-stu-id="59e96-157">hello URLs look like: `https://<storageaccount>.blob.core.windows.net/<containerName>/`.</span></span> <span data-ttu-id="59e96-158">Pokud už znáte hello název účtu a kontejneru úložiště, můžete právě nahradit hello informací mezi hello závorky toocreate adresu URL.</span><span class="sxs-lookup"><span data-stu-id="59e96-158">If you already know hello storage account and container name, you can just replace hello information between hello brackets toocreate your URL.</span></span> 

<span data-ttu-id="59e96-159">Můžete použít hello portál Azure nebo Azure Powershell tooget hello adresa URL:</span><span class="sxs-lookup"><span data-stu-id="59e96-159">You can use hello Azure portal or Azure Powershell tooget hello URL:</span></span>

* <span data-ttu-id="59e96-160">**Portál**: klikněte na tlačítko hello  **>**  pro **další služby** > **účty úložiště**  >   *účet úložiště* > **objekty BLOB** a váš zdrojový soubor virtuálního pevného disku je pravděpodobně v hello **virtuální pevné disky** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="59e96-160">**Portal**: Click hello **>** for **More services** > **Storage accounts** > *storage account* > **Blobs** and your source VHD file is probably in hello **vhds** container.</span></span> <span data-ttu-id="59e96-161">Klikněte na tlačítko **vlastnosti** hello kontejneru a kopírovat text hello s názvem bez přípony **URL**.</span><span class="sxs-lookup"><span data-stu-id="59e96-161">Click **Properties** for hello container, and copy hello text labeled **URL**.</span></span> <span data-ttu-id="59e96-162">Budete potřebovat kontejnery obou hello zdrojové a cílové adresy URL hello.</span><span class="sxs-lookup"><span data-stu-id="59e96-162">You'll need hello URLs of both hello source and destination containers.</span></span> 
* <span data-ttu-id="59e96-163">**Prostředí PowerShell**: použití [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) tooget hello informace pro virtuální počítač s názvem **Můjvp** ve skupině prostředků hello **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="59e96-163">**Powershell**: Use [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) tooget hello information for VM named **myVM** in hello resource group **myResourceGroup**.</span></span> <span data-ttu-id="59e96-164">Ve výsledcích hello Hledat v hello **profilu úložiště** části hello **Uri virtuálního pevného disku**.</span><span class="sxs-lookup"><span data-stu-id="59e96-164">In hello results, look in hello **Storage profile** section for hello **Vhd Uri**.</span></span> <span data-ttu-id="59e96-165">první část Hello hello identifikátor Uri je hello URL toohello kontejneru a poslední část hello hello název virtuálního pevného disku operačního systému pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="59e96-165">hello first part of hello Uri is hello URL toohello container and hello last part is hello OS VHD name for hello VM.</span></span>

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
``` 

## <a name="get-hello-storage-access-keys"></a><span data-ttu-id="59e96-166">Získání přístupových klíčů k úložišti hello</span><span class="sxs-lookup"><span data-stu-id="59e96-166">Get hello storage access keys</span></span>
<span data-ttu-id="59e96-167">Najít hello přístupové klíče pro hello zdrojové a cílové účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="59e96-167">Find hello access keys for hello source and destination storage accounts.</span></span> <span data-ttu-id="59e96-168">Další informace o přístupových klíčů najdete v tématu [účty Azure storage](../../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="59e96-168">For more information about access keys, see [About Azure storage accounts](../../storage/common/storage-create-storage-account.md).</span></span>

* <span data-ttu-id="59e96-169">**Portál**: klikněte na tlačítko **další služby** > **účty úložiště** > *účet úložiště*  >  **Přístupové klíče**.</span><span class="sxs-lookup"><span data-stu-id="59e96-169">**Portal**: Click **More services** > **Storage accounts** > *storage account* > **Access keys**.</span></span> <span data-ttu-id="59e96-170">Kopírování hello klíč označený jako **key1**.</span><span class="sxs-lookup"><span data-stu-id="59e96-170">Copy hello key labeled as **key1**.</span></span>
* <span data-ttu-id="59e96-171">**Prostředí PowerShell**: použití [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) tooget hello úložiště klíč pro účet úložiště hello **můj_účet_úložiště** ve skupině prostředků hello  **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="59e96-171">**Powershell**: Use [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) tooget hello storage key for hello storage account **mystorageaccount** in hello resource group **myResourceGroup**.</span></span> <span data-ttu-id="59e96-172">Kopírování hello klíč s názvem bez přípony **key1**.</span><span class="sxs-lookup"><span data-stu-id="59e96-172">Copy hello key labeled **key1**.</span></span>

```powershell
Get-AzureRmStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup
```

### <a name="copy-hello-vhd"></a><span data-ttu-id="59e96-173">Zkopírujte hello virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="59e96-173">Copy hello VHD</span></span>
<span data-ttu-id="59e96-174">Můžete zkopírovat soubory mezi účty úložiště pomocí nástroje AzCopy.</span><span class="sxs-lookup"><span data-stu-id="59e96-174">You can copy files between storage accounts using AzCopy.</span></span> <span data-ttu-id="59e96-175">Pro cílový kontejner hello Pokud hello zadaný kontejner neexistuje, bude vytvořena pro vás.</span><span class="sxs-lookup"><span data-stu-id="59e96-175">For hello destination container, if hello specified container doesn't exist, it will be created for you.</span></span> 

<span data-ttu-id="59e96-176">toouse AzCopy, otevřete příkazový řádek na místním počítači a přejděte toohello složku, kde je nainstalován nástroj AzCopy.</span><span class="sxs-lookup"><span data-stu-id="59e96-176">toouse AzCopy, open a command prompt on your local machine and navigate toohello folder where AzCopy is installed.</span></span> <span data-ttu-id="59e96-177">Je podobné příliš*C:\Program Files (x86) \Microsoft SDKs\Azure\AzCopy*.</span><span class="sxs-lookup"><span data-stu-id="59e96-177">It will be similar too*C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy*.</span></span> 

<span data-ttu-id="59e96-178">toocopy všechny hello souborů v rámci kontejneru, použijte hello **/S** přepínače.</span><span class="sxs-lookup"><span data-stu-id="59e96-178">toocopy all of hello files within a container, you use hello **/S** switch.</span></span> <span data-ttu-id="59e96-179">To může být použité toocopy hello virtuálního pevného disku operačního systému a všechny hello datových disků, pokud jsou ve stejném kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="59e96-179">This can be used toocopy hello OS VHD and all of hello data disks if they are in hello same container.</span></span> <span data-ttu-id="59e96-180">Tento příklad ukazuje, jak toocopy všechny hello soubory v kontejneru hello **mysourcecontainer** v účtu úložiště **mysourcestorageaccount** toohello kontejneru **mydestinationcontainer**  v hello **mydestinationstorageaccount** účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="59e96-180">This example shows how toocopy all of hello files in hello container **mysourcecontainer** in storage account **mysourcestorageaccount** toohello container **mydestinationcontainer** in hello **mydestinationstorageaccount** storage account.</span></span> <span data-ttu-id="59e96-181">Nahraďte hello názvy účtů úložiště hello a kontejnery vlastními.</span><span class="sxs-lookup"><span data-stu-id="59e96-181">Replace hello names of hello storage accounts and containers with your own.</span></span> <span data-ttu-id="59e96-182">Nahraďte `<sourceStorageAccountKey1>` a `<destinationStorageAccountKey1>` s vlastními klíči.</span><span class="sxs-lookup"><span data-stu-id="59e96-182">Replace `<sourceStorageAccountKey1>` and `<destinationStorageAccountKey1>` with your own keys.</span></span>

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
    /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
    /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

<span data-ttu-id="59e96-183">Pokud chcete pouze toocopy konkrétní virtuální pevný disk v kontejneru s více soubory, můžete také zadat název souboru hello pomocí přepínače /Pattern hello.</span><span class="sxs-lookup"><span data-stu-id="59e96-183">If you only want toocopy a specific VHD in a container with multiple files, you can also specify hello file name using hello /Pattern switch.</span></span> <span data-ttu-id="59e96-184">V tomto příkladu pouze hello soubor s názvem **myFileName.vhd** budou zkopírovány.</span><span class="sxs-lookup"><span data-stu-id="59e96-184">In this example, only hello file named **myFileName.vhd** will be copied.</span></span>

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
  /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
  /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
  /Pattern:myFileName.vhd
```


<span data-ttu-id="59e96-185">Po dokončení, zobrazí se zpráva, která vypadá podobně jako:</span><span class="sxs-lookup"><span data-stu-id="59e96-185">When it is finished, you will get a message that looks something like:</span></span>

```
Finished 2 of total 2 file(s).
[2016/10/07 17:37:41] Transfer summary:
-----------------
Total files transferred: 2
Transfer successfully:   2
Transfer skipped:        0
Transfer failed:         0
Elapsed time:            00.00:13:07
```

### <a name="troubleshooting"></a><span data-ttu-id="59e96-186">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="59e96-186">Troubleshooting</span></span>
* <span data-ttu-id="59e96-187">Při použití nástroje AZCopy, pokud se zobrazí chyba hello "Serveru se nezdařilo požadavek tooauthenticate hello", zkontrolujte, zda je hodnota hello hello autorizační hlavičky formátu správně včetně hello podpis.</span><span class="sxs-lookup"><span data-stu-id="59e96-187">When you use AZCopy, if you see hello error "Server failed tooauthenticate hello request", make sure hello value of hello Authorization header is formed correctly including hello signature.</span></span> <span data-ttu-id="59e96-188">Pokud používáte 2 klíč nebo klíč hello sekundární úložiště, zkuste použít klíč hello primární nebo 1. úložiště.</span><span class="sxs-lookup"><span data-stu-id="59e96-188">If you are using Key 2 or hello secondary storage key, try using hello primary or 1st storage key.</span></span>

## <a name="create-hello-new-vm"></a><span data-ttu-id="59e96-189">Vytvoření nového virtuálního počítače hello</span><span class="sxs-lookup"><span data-stu-id="59e96-189">Create hello new VM</span></span> 

<span data-ttu-id="59e96-190">Budete potřebovat toocreate sítě a jiných toobe prostředky virtuálních počítačů používané hello nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="59e96-190">You need toocreate networking and other VM resources toobe used by hello new VM.</span></span>

### <a name="create-hello-subnet-and-vnet"></a><span data-ttu-id="59e96-191">Vytvoření podsítě hello a virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="59e96-191">Create hello subNet and vNet</span></span>

<span data-ttu-id="59e96-192">Vytvoření hello virtuální síť a podsíť hello [virtuální sítě](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="59e96-192">Create hello vNet and subNet of hello [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="59e96-193">Vytvořte podsíť hello.</span><span class="sxs-lookup"><span data-stu-id="59e96-193">Create hello subNet.</span></span> <span data-ttu-id="59e96-194">Tento příklad vytvoří podsíť s názvem **mySubNet**, ve skupině prostředků hello **myResourceGroup**, a nastaví hello předpona adresy podsítě příliš**10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="59e96-194">This example creates a subnet named **mySubNet**, in hello resource group **myResourceGroup**, and sets hello subnet address prefix too**10.0.0.0/24**.</span></span>
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="59e96-195">Vytvoření sítě vNet hello.</span><span class="sxs-lookup"><span data-stu-id="59e96-195">Create hello vNet.</span></span> <span data-ttu-id="59e96-196">Tento příklad sady hello toobe název virtuální sítě **myVnetName**, hello umístění příliš**západní USA**, a příliš hello předpona adresy pro virtuální síť hello**10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="59e96-196">This example sets hello virtual network name toobe **myVnetName**, hello location too**West US**, and hello address prefix for hello virtual network too**10.0.0.0/16**.</span></span> 
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-nic"></a><span data-ttu-id="59e96-197">Vytvoření veřejné IP adresy a síťové karty</span><span class="sxs-lookup"><span data-stu-id="59e96-197">Create a public IP address and NIC</span></span>
<span data-ttu-id="59e96-198">tooenable komunikaci s hello virtuálním počítačem ve virtuální síti hello, musíte [veřejnou IP adresu](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) a síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="59e96-198">tooenable communication with hello virtual machine in hello virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="59e96-199">Vytvoření veřejné IP adresy hello.</span><span class="sxs-lookup"><span data-stu-id="59e96-199">Create hello public IP.</span></span> <span data-ttu-id="59e96-200">V tomto příkladu je název hello veřejné IP adresy nastavit příliš**myIP**.</span><span class="sxs-lookup"><span data-stu-id="59e96-200">In this example, hello public IP address name is set too**myIP**.</span></span>
   
    ```powershell
    $ipName = "myIP"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="59e96-201">Vytvoření hello síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="59e96-201">Create hello NIC.</span></span> <span data-ttu-id="59e96-202">V tomto příkladu název hello síťový adaptér se nastaví příliš**myNicName**.</span><span class="sxs-lookup"><span data-stu-id="59e96-202">In this example, hello NIC name is set too**myNicName**.</span></span>
   
    ```powershell
    $nicName = "myNicName"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-hello-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="59e96-203">Vytvořte skupinu zabezpečení sítě hello a pravidlo protokolu RDP</span><span class="sxs-lookup"><span data-stu-id="59e96-203">Create hello network security group and an RDP rule</span></span>
<span data-ttu-id="59e96-204">možnost toolog toobe v tooyour virtuálního počítače pomocí protokolu RDP, je nutné toohave pravidlo zabezpečení, která umožňuje přístup RDP na portu 3389.</span><span class="sxs-lookup"><span data-stu-id="59e96-204">toobe able toolog in tooyour VM using RDP, you need toohave an security rule that allows RDP access on port 3389.</span></span> <span data-ttu-id="59e96-205">Protože specializuje hello virtuálního pevného disku pro dobrý den, kdy byl vytvořen nový virtuální počítač ze stávajícího virtuálního počítače, po vytvoření hello virtuálního počítače je můžete použít existující účet z hello zdrojový virtuální počítač, který měl oprávnění toolog na použití protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="59e96-205">Because hello VHD for hello new VM was created from an existing specialized VM, after hello VM is created you can use an existing account from hello source virtual machine that had permission toolog on using RDP.</span></span>
<span data-ttu-id="59e96-206">Tento příklad sady hello NSG název příliš**myNsg** a hello RDP název pravidla příliš**myRdpRule**.</span><span class="sxs-lookup"><span data-stu-id="59e96-206">This example sets hello NSG name too**myNsg** and hello RDP rule name too**myRdpRule**.</span></span>

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

<span data-ttu-id="59e96-207">Další informace o koncových bodů a pravidla NSG najdete v tématu [otevřít porty tooa virtuálních počítačů v Azure pomocí prostředí PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="59e96-207">For more information about endpoints and NSG rules, see [Opening ports tooa VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

### <a name="set-hello-vm-name-and-size"></a><span data-ttu-id="59e96-208">Nastavte název virtuálního počítače hello a velikosti</span><span class="sxs-lookup"><span data-stu-id="59e96-208">Set hello VM name and size</span></span>

<span data-ttu-id="59e96-209">Tento příklad sady hello název virtuálního počítače příliš "Můjvp" a hello virtuálních počítačů velikost příliš "Standard_A2".</span><span class="sxs-lookup"><span data-stu-id="59e96-209">This example sets hello VM name too"myVM" and hello VM size too"Standard_A2".</span></span>
```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-hello-nic"></a><span data-ttu-id="59e96-210">Přidat hello síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="59e96-210">Add hello NIC</span></span>
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    
    
### <a name="configure-hello-os-disk"></a><span data-ttu-id="59e96-211">Konfigurace disku hello operačního systému</span><span class="sxs-lookup"><span data-stu-id="59e96-211">Configure hello OS disk</span></span>

1. <span data-ttu-id="59e96-212">Nastavte hello identifikátor URI pro hello virtuálního pevného disku, který jste nahráli nebo zkopírovat.</span><span class="sxs-lookup"><span data-stu-id="59e96-212">Set hello URI for hello VHD that you uploaded or copied.</span></span> <span data-ttu-id="59e96-213">V tomto příkladu hello soubor virtuálního pevného disku s názvem **myOsDisk.vhd** je uložen v účtu úložiště s názvem **Můj_účet_úložiště** v kontejneru nazvaném **Můj_kontejner**.</span><span class="sxs-lookup"><span data-stu-id="59e96-213">In this example, hello VHD file named **myOsDisk.vhd** is kept in a storage account named **myStorageAccount** in a container named **myContainer**.</span></span>

    ```powershell
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    ```
2. <span data-ttu-id="59e96-214">Přidání disku hello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="59e96-214">Add hello OS disk.</span></span> <span data-ttu-id="59e96-215">V tomto příkladu když je vytvořen hello disk operačního systému, hello termín "osDisk" je appened toohello virtuálních počítačů název toocreate hello operačního systému disku název.</span><span class="sxs-lookup"><span data-stu-id="59e96-215">In this example, when hello OS disk is created, hello term "osDisk" is appened toohello VM name toocreate hello OS disk name.</span></span> <span data-ttu-id="59e96-216">Tento příklad také určuje, že tento virtuální pevný disk systému Windows musí být připojené toohello virtuální počítač jako disk hello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="59e96-216">This example also specifies that this Windows-based VHD should be attached toohello VM as hello OS disk.</span></span>
    
    ```powershell
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
    ```

<span data-ttu-id="59e96-217">Volitelné: Pokud máte datových disků tohoto toobe nutné připojit toohello virtuálních počítačů, přidejte hello datových disků pomocí adres URL hello dat virtuální pevné disky a hello odpovídající logické jednotky (LUN).</span><span class="sxs-lookup"><span data-stu-id="59e96-217">Optional: If you have data disks that need toobe attached toohello VM, add hello data disks by using hello URLs of data VHDs and hello appropriate Logical Unit Number (Lun).</span></span>

```powershell
$dataDiskName = $vmName + "dataDisk"
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 1 -CreateOption attach
```

<span data-ttu-id="59e96-218">Pokud používáte účet úložiště, hello data a adresy URL disku operačního systému vypadat přibližně takto: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`.</span><span class="sxs-lookup"><span data-stu-id="59e96-218">When using a storage account, hello data and operating system disk URLs look something like this: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`.</span></span> <span data-ttu-id="59e96-219">To můžete najít na portálu hello procházení toohello cílového úložiště kontejneru, kliknutím na tlačítko hello operačního systému nebo data virtuálního pevného disku, který jste zkopírovali, a pak kopírování hello obsah adresy URL hello.</span><span class="sxs-lookup"><span data-stu-id="59e96-219">You can find this on hello portal by browsing toohello target storage container, clicking hello operating system or data VHD that was copied, and then copying hello contents of hello URL.</span></span>


### <a name="complete-hello-vm"></a><span data-ttu-id="59e96-220">Dokončení hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="59e96-220">Complete hello VM</span></span> 

<span data-ttu-id="59e96-221">Vytvoření hello virtuálního počítače pomocí hello konfigurace, které jsme právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="59e96-221">Create hello VM using hello configurations that we just created.</span></span>

```powershell
#Create hello new VM
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

<span data-ttu-id="59e96-222">Pokud tento příkaz byl úspěšný, zobrazí se výstup takto:</span><span class="sxs-lookup"><span data-stu-id="59e96-222">If this command was successful, you'll see output like this:</span></span>

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-hello-vm-was-created"></a><span data-ttu-id="59e96-223">Ověřte, že hello, ke které byl virtuální počítač vytvořen</span><span class="sxs-lookup"><span data-stu-id="59e96-223">Verify that hello VM was created</span></span>
<span data-ttu-id="59e96-224">Měli byste vidět hello nově vytvořený virtuální počítač buď v hello [portál Azure](https://portal.azure.com)v části **Procházet** > **virtuální počítače**, nebo pomocí prostředí PowerShell následující hello příkazy:</span><span class="sxs-lookup"><span data-stu-id="59e96-224">You should see hello newly created VM either in hello [Azure portal](https://portal.azure.com), under **Browse** > **Virtual machines**, or by using hello following PowerShell commands:</span></span>

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $rgName
$vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="59e96-225">Další kroky</span><span class="sxs-lookup"><span data-stu-id="59e96-225">Next steps</span></span>
<span data-ttu-id="59e96-226">toosign v tooyour nový virtuální počítač, procházet toohello virtuálních počítačů v hello [portál](https://portal.azure.com), klikněte na tlačítko **Connect**a soubor Remote Desktop RDP otevřete hello.</span><span class="sxs-lookup"><span data-stu-id="59e96-226">toosign in tooyour new virtual machine, browse toohello VM in hello [portal](https://portal.azure.com), click **Connect**, and open hello Remote Desktop RDP file.</span></span> <span data-ttu-id="59e96-227">Pomocí přihlašovacích údajů účtu hello z vašeho původního toosign virtuálního počítače v tooyour nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="59e96-227">Use hello account credentials of your original virtual machine toosign in tooyour new virtual machine.</span></span> <span data-ttu-id="59e96-228">Další informace najdete v tématu [jak se tooconnect a protokol na tooan Azure virtuální počítač, systém Windows spuštěný](connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="59e96-228">For more information, see [How tooconnect and log on tooan Azure virtual machine running Windows](connect-logon.md).</span></span>

