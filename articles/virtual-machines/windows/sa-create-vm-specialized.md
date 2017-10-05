---
title: "Vytvoření virtuálního počítače z disku specializované v Azure | Microsoft Docs"
description: "Vytvoření nového virtuálního počítače připojením specializované nespravované disku, v modelu nasazení Resource Manager."
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
ms.openlocfilehash: 974d89aa96cba94fedfd1acbaf4f1d30ac8e6257
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-vm-from-a-specialized-vhd-in-a-storage-account"></a><span data-ttu-id="3b589-103">Vytvoření virtuálního počítače z specializované virtuálního pevného disku v účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="3b589-103">Create a VM from a specialized VHD in a storage account</span></span>

<span data-ttu-id="3b589-104">Vytvoření nového virtuálního počítače připojením specializované nespravované disk jako disk operačního systému pomocí prostředí Powershell.</span><span class="sxs-lookup"><span data-stu-id="3b589-104">Create a new VM by attaching a specialized unmanaged disk as the OS disk using Powershell.</span></span> <span data-ttu-id="3b589-105">Specializované disk je kopie virtuálního pevného disku z existující virtuální počítač, který uchovává uživatelské účty, aplikace a další data o stavu z vašeho původního virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3b589-105">A specialized disk is a copy of VHD from an existing VM that maintains the user accounts, applications and other state data from your original VM.</span></span> 

<span data-ttu-id="3b589-106">Máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="3b589-106">You have two options:</span></span>
* [<span data-ttu-id="3b589-107">Nahrání virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="3b589-107">Upload a VHD</span></span>](create-vm-specialized.md#option-1-upload-a-specialized-vhd)
* [<span data-ttu-id="3b589-108">Zkopírujte virtuální pevný disk existující virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="3b589-108">Copy the VHD of an existing Azure VM</span></span>](create-vm-specialized.md#option-2-copy-an-existing-azure-vm)

## <a name="before-you-begin"></a><span data-ttu-id="3b589-109">Než začnete</span><span class="sxs-lookup"><span data-stu-id="3b589-109">Before you begin</span></span>
<span data-ttu-id="3b589-110">Pokud používáte prostředí PowerShell, ujistěte se, že máte nejnovější verzi modulu prostředí AzureRM.Compute PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3b589-110">If you use PowerShell, make sure that you have the latest version of the AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="3b589-111">Spusťte následující příkaz k její instalaci.</span><span class="sxs-lookup"><span data-stu-id="3b589-111">Run the following command to install it.</span></span>

```powershell
Install-Module AzureRM.Compute 
```
<span data-ttu-id="3b589-112">Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3b589-112">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="option-1-upload-a-specialized-vhd"></a><span data-ttu-id="3b589-113">Možnost 1: Nahrát specializované virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="3b589-113">Option 1: Upload a specialized VHD</span></span>

<span data-ttu-id="3b589-114">Můžete nahrát virtuální pevný disk z virtuálního počítače specializované vytvořené pomocí místní virtualizace nástroje, jako je technologie Hyper-V nebo na virtuální počítač exportovat z jiného cloudu.</span><span class="sxs-lookup"><span data-stu-id="3b589-114">You can upload the VHD from a specialized VM created with an on-premises virtualization tool, like Hyper-V, or a VM exported from another cloud.</span></span>

### <a name="prepare-the-vm"></a><span data-ttu-id="3b589-115">Příprava virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="3b589-115">Prepare the VM</span></span>
<span data-ttu-id="3b589-116">Můžete nahrát specializované VHD, který byl vytvořen pomocí místní počítač nebo virtuální pevný disk exportovaný z jiného cloudu.</span><span class="sxs-lookup"><span data-stu-id="3b589-116">You can upload a specialized VHD that was created using an on-premises VM or a VHD exported from another cloud.</span></span> <span data-ttu-id="3b589-117">Specializované virtuálního pevného disku uchovává uživatelské účty, aplikace a další data o stavu z vašeho původního virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3b589-117">A specialized VHD maintains the user accounts, applications and other state data from your original VM.</span></span> <span data-ttu-id="3b589-118">Pokud máte v úmyslu použít virtuální pevný disk jako-je chcete vytvořit nový virtuální počítač, zkontrolujte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="3b589-118">If you intend to use the VHD as-is to create a new VM, ensure the following steps are completed.</span></span> 
  
  * <span data-ttu-id="3b589-119">[Příprava virtuálního pevného disku Windows nahrát do Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3b589-119">[Prepare a Windows VHD to upload to Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="3b589-120">**Nechcete** generalize virtuální počítač pomocí nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="3b589-120">**Do not** generalize the VM using Sysprep.</span></span>
  * <span data-ttu-id="3b589-121">Odeberte všechny hosta virtualizačních nástrojů a agentů, které jsou nainstalovány do virtuálního počítače (tj. nástroje VMware).</span><span class="sxs-lookup"><span data-stu-id="3b589-121">Remove any guest virtualization tools and agents that are installed on the VM (i.e. VMware tools).</span></span>
  * <span data-ttu-id="3b589-122">Zajistěte, aby že virtuální počítač nakonfigurovaný tak, aby jeho IP adresu a nastavení DNS pomocí protokolu DHCP pro vyžádání obsahu.</span><span class="sxs-lookup"><span data-stu-id="3b589-122">Ensure the VM is configured to pull its IP address and DNS settings via DHCP.</span></span> <span data-ttu-id="3b589-123">To zajistí, že server získá IP adresu v rámci virtuální sítě, při spuštění.</span><span class="sxs-lookup"><span data-stu-id="3b589-123">This ensures that the server obtains an IP address within the VNet when it starts up.</span></span> 


### <a name="get-the-storage-account"></a><span data-ttu-id="3b589-124">Získat účet úložiště</span><span class="sxs-lookup"><span data-stu-id="3b589-124">Get the storage account</span></span>
<span data-ttu-id="3b589-125">Potřebujete účet úložiště v Azure k ukládání nahrané image virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3b589-125">You need a storage account in Azure to store the uploaded VM image.</span></span> <span data-ttu-id="3b589-126">Můžete použít existující účet úložiště, nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="3b589-126">You can either use an existing storage account or create a new one.</span></span> 

<span data-ttu-id="3b589-127">Chcete-li zobrazit účty úložiště k dispozici, zadejte:</span><span class="sxs-lookup"><span data-stu-id="3b589-127">To show the available storage accounts, type:</span></span>

```powershell
Get-AzureRmStorageAccount
```

<span data-ttu-id="3b589-128">Pokud chcete použít existující účet úložiště, pokračujte [nahrajte image virtuálního počítače](#upload-the-vm-vhd-to-your-storage-account) části.</span><span class="sxs-lookup"><span data-stu-id="3b589-128">If you want to use an existing storage account, proceed to the [Upload the VM image](#upload-the-vm-vhd-to-your-storage-account) section.</span></span>

<span data-ttu-id="3b589-129">Pokud potřebujete vytvořit účet úložiště, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="3b589-129">If you need to create a storage account, follow these steps:</span></span>

1. <span data-ttu-id="3b589-130">Je třeba na název skupiny prostředků, kde by měl být vytvořen účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="3b589-130">You need the name of the resource group where the storage account should be created.</span></span> <span data-ttu-id="3b589-131">Chcete-li zjistit všechny skupiny prostředků, které jsou v rámci vašeho předplatného, zadejte:</span><span class="sxs-lookup"><span data-stu-id="3b589-131">To find out all the resource groups that are in your subscription, type:</span></span>
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    <span data-ttu-id="3b589-132">Chcete-li vytvořit skupinu prostředků s názvem **myResourceGroup** v **západní USA** oblast, zadejte:</span><span class="sxs-lookup"><span data-stu-id="3b589-132">To create a resource group named **myResourceGroup** in the **West US** region, type:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. <span data-ttu-id="3b589-133">Vytvořit účet úložiště s názvem **můj_účet_úložiště** v této skupině prostředků s použitím [AzureRmStorageAccount nový](/powershell/module/azurerm.storage/new-azurermstorageaccount) rutiny:</span><span class="sxs-lookup"><span data-stu-id="3b589-133">Create a storage account named **mystorageaccount** in this resource group by using the [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span></span>
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
   
### <a name="upload-the-vhd-to-your-storage-account"></a><span data-ttu-id="3b589-134">Nahrání virtuálního pevného disku do účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="3b589-134">Upload the VHD to your storage account</span></span>
<span data-ttu-id="3b589-135">Použití [přidat AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) rutiny Odeslat bitovou kopii do kontejneru v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="3b589-135">Use the [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet to upload the image to a container in your storage account.</span></span> <span data-ttu-id="3b589-136">Tento příklad nahrávání souboru **myVHD.vhd** z `"C:\Users\Public\Documents\Virtual hard disks\"` na účet úložiště s názvem **můj_účet_úložiště** v **myResourceGroup** skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="3b589-136">This example uploads the file **myVHD.vhd** from `"C:\Users\Public\Documents\Virtual hard disks\"` to a storage account named **mystorageaccount** in the **myResourceGroup** resource group.</span></span> <span data-ttu-id="3b589-137">Soubor se umístí do kontejner s názvem **můj_kontejner** a nový název souboru bude **myUploadedVHD.vhd**.</span><span class="sxs-lookup"><span data-stu-id="3b589-137">The file will be placed into the container named **mycontainer** and the new file name will be **myUploadedVHD.vhd**.</span></span>

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


<span data-ttu-id="3b589-138">Pokud bylo úspěšné, můžete získat odpovědi, která vypadá podobně jako tento:</span><span class="sxs-lookup"><span data-stu-id="3b589-138">If successful, you get a response that looks similar to this:</span></span>

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

<span data-ttu-id="3b589-139">V závislosti na připojení k síti a velikost souboru virtuálního pevného disku tento příkaz může trvat nějakou dobu pro dokončení.</span><span class="sxs-lookup"><span data-stu-id="3b589-139">Depending on your network connection and the size of your VHD file, this command may take a while to complete.</span></span>


## <a name="option-2-copy-the-vhd-from-an-existing-azure-vm"></a><span data-ttu-id="3b589-140">Možnost 2: Zkopírujte virtuální pevný disk z existujícího virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="3b589-140">Option 2: Copy the VHD from an existing Azure VM</span></span>

<span data-ttu-id="3b589-141">Virtuální pevný disk můžete zkopírovat na jiný účet úložiště pro použití při vytvoření nového virtuálního počítače duplicitní.</span><span class="sxs-lookup"><span data-stu-id="3b589-141">You can copy a VHD to another storage account to use when creating a new, duplicate VM.</span></span>

### <a name="before-you-begin"></a><span data-ttu-id="3b589-142">Než začnete</span><span class="sxs-lookup"><span data-stu-id="3b589-142">Before you begin</span></span>
<span data-ttu-id="3b589-143">Ujistěte se, že jste:</span><span class="sxs-lookup"><span data-stu-id="3b589-143">Make sure that you:</span></span>

* <span data-ttu-id="3b589-144">Neobsahuje informace o **zdrojové a cílové účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="3b589-144">Have information about the **source and destination storage accounts**.</span></span> <span data-ttu-id="3b589-145">Pro zdrojový virtuální počítač je potřeba mít názvy účtů a kontejner úložiště.</span><span class="sxs-lookup"><span data-stu-id="3b589-145">For the source VM, you need to have the storage account and container names.</span></span> <span data-ttu-id="3b589-146">Obvykle bude název kontejneru **virtuální pevné disky**.</span><span class="sxs-lookup"><span data-stu-id="3b589-146">Usually, the container name will be **vhds**.</span></span> <span data-ttu-id="3b589-147">Také musíte mít cílový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="3b589-147">You also need to have a destination storage account.</span></span> <span data-ttu-id="3b589-148">Pokud jste již nemáte, můžete vytvořit pomocí buď na portálu (**více služeb** > účty úložiště > Přidat) nebo pomocí [AzureRmStorageAccount nový](/powershell/module/azurerm.storage/new-azurermstorageaccount) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3b589-148">If you don't already have one, you can create one using either the portal (**More Services** > Storage accounts > Add) or using the [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet.</span></span> 
* <span data-ttu-id="3b589-149">Stáhli a nainstalovali [nástroj AzCopy](../../storage/common/storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="3b589-149">Have downloaded and installed the [AzCopy tool](../../storage/common/storage-use-azcopy.md).</span></span> 

### <a name="deallocate-the-vm"></a><span data-ttu-id="3b589-150">Zrušit přidělení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="3b589-150">Deallocate the VM</span></span>
<span data-ttu-id="3b589-151">Zrušit přidělení virtuálního počítače, což uvolní virtuální pevný disk, který se má zkopírovat.</span><span class="sxs-lookup"><span data-stu-id="3b589-151">Deallocate the VM, which frees up the VHD to be copied.</span></span> 

* <span data-ttu-id="3b589-152">**Portál**: klikněte na tlačítko **virtuální počítače** > **Můjvp** > Zastavit</span><span class="sxs-lookup"><span data-stu-id="3b589-152">**Portal**: Click **Virtual machines** > **myVM** > Stop</span></span>
* <span data-ttu-id="3b589-153">**Prostředí PowerShell**: použití [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) zastavit (zrušit přidělení) virtuálního počítače s názvem **Můjvp** ve skupině prostředků **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="3b589-153">**Powershell**: Use [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) to stop (deallocate) the VM named **myVM** in resource group **myResourceGroup**.</span></span>

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
```

<span data-ttu-id="3b589-154">**Stav** pro virtuální počítač ve službě Azure portal změní z **Zastaveno** k **zastaveném (nepřiřazeném)**.</span><span class="sxs-lookup"><span data-stu-id="3b589-154">The **Status** for the VM in the Azure portal changes from **Stopped** to **Stopped (deallocated)**.</span></span>

### <a name="get-the-storage-account-urls"></a><span data-ttu-id="3b589-155">Získání adres URL účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="3b589-155">Get the storage account URLs</span></span>
<span data-ttu-id="3b589-156">Je třeba účty úložiště zdrojové a cílové adresy URL.</span><span class="sxs-lookup"><span data-stu-id="3b589-156">You need the URLs of the source and destination storage accounts.</span></span> <span data-ttu-id="3b589-157">Jako adresy URL vzhledu: `https://<storageaccount>.blob.core.windows.net/<containerName>/`.</span><span class="sxs-lookup"><span data-stu-id="3b589-157">The URLs look like: `https://<storageaccount>.blob.core.windows.net/<containerName>/`.</span></span> <span data-ttu-id="3b589-158">Pokud znáte název účtu a kontejneru úložiště, můžete nahradit pouze informace k vytvoření adresu URL do závorek.</span><span class="sxs-lookup"><span data-stu-id="3b589-158">If you already know the storage account and container name, you can just replace the information between the brackets to create your URL.</span></span> 

<span data-ttu-id="3b589-159">Portál Azure nebo Azure Powershell můžete použít k získání adresy URL:</span><span class="sxs-lookup"><span data-stu-id="3b589-159">You can use the Azure portal or Azure Powershell to get the URL:</span></span>

* <span data-ttu-id="3b589-160">**Portál**: klikněte  **>**  pro **další služby** > **účty úložiště**  >   *účet úložiště* > **objekty BLOB** a váš zdrojový soubor virtuálního pevného disku je pravděpodobně v **virtuální pevné disky** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="3b589-160">**Portal**: Click the **>** for **More services** > **Storage accounts** > *storage account* > **Blobs** and your source VHD file is probably in the **vhds** container.</span></span> <span data-ttu-id="3b589-161">Klikněte na tlačítko **vlastnosti** kontejneru a zkopírujte text s názvem bez přípony **URL**.</span><span class="sxs-lookup"><span data-stu-id="3b589-161">Click **Properties** for the container, and copy the text labeled **URL**.</span></span> <span data-ttu-id="3b589-162">Budete potřebovat adresy URL zdrojového a cílového kontejnery.</span><span class="sxs-lookup"><span data-stu-id="3b589-162">You'll need the URLs of both the source and destination containers.</span></span> 
* <span data-ttu-id="3b589-163">**Prostředí PowerShell**: použití [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) získání informací o pro virtuální počítač s názvem **Můjvp** ve skupině prostředků **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="3b589-163">**Powershell**: Use [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) to get the information for VM named **myVM** in the resource group **myResourceGroup**.</span></span> <span data-ttu-id="3b589-164">Ve výsledcích vyhledejte v **profilu úložiště** část **Uri virtuálního pevného disku**.</span><span class="sxs-lookup"><span data-stu-id="3b589-164">In the results, look in the **Storage profile** section for the **Vhd Uri**.</span></span> <span data-ttu-id="3b589-165">První část identifikátoru Uri je adresa URL ke kontejneru a poslední část je název virtuálního pevného disku operačního systému pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3b589-165">The first part of the Uri is the URL to the container and the last part is the OS VHD name for the VM.</span></span>

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
``` 

## <a name="get-the-storage-access-keys"></a><span data-ttu-id="3b589-166">Získat přístupové klíče k úložišti</span><span class="sxs-lookup"><span data-stu-id="3b589-166">Get the storage access keys</span></span>
<span data-ttu-id="3b589-167">Najděte přístupové klíče pro zdrojové a cílové účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="3b589-167">Find the access keys for the source and destination storage accounts.</span></span> <span data-ttu-id="3b589-168">Další informace o přístupových klíčů najdete v tématu [účty Azure storage](../../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="3b589-168">For more information about access keys, see [About Azure storage accounts](../../storage/common/storage-create-storage-account.md).</span></span>

* <span data-ttu-id="3b589-169">**Portál**: klikněte na tlačítko **další služby** > **účty úložiště** > *účet úložiště*  >  **Přístupové klíče**.</span><span class="sxs-lookup"><span data-stu-id="3b589-169">**Portal**: Click **More services** > **Storage accounts** > *storage account* > **Access keys**.</span></span> <span data-ttu-id="3b589-170">Zkopírujte klíč označený jako **key1**.</span><span class="sxs-lookup"><span data-stu-id="3b589-170">Copy the key labeled as **key1**.</span></span>
* <span data-ttu-id="3b589-171">**Prostředí PowerShell**: použití [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) k získání klíče úložiště pro účet úložiště **můj_účet_úložiště** ve skupině prostředků **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="3b589-171">**Powershell**: Use [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) to get the storage key for the storage account **mystorageaccount** in the resource group **myResourceGroup**.</span></span> <span data-ttu-id="3b589-172">Zkopírujte klíč s názvem bez přípony **key1**.</span><span class="sxs-lookup"><span data-stu-id="3b589-172">Copy the key labeled **key1**.</span></span>

```powershell
Get-AzureRmStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup
```

### <a name="copy-the-vhd"></a><span data-ttu-id="3b589-173">Zkopírujte virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="3b589-173">Copy the VHD</span></span>
<span data-ttu-id="3b589-174">Můžete zkopírovat soubory mezi účty úložiště pomocí nástroje AzCopy.</span><span class="sxs-lookup"><span data-stu-id="3b589-174">You can copy files between storage accounts using AzCopy.</span></span> <span data-ttu-id="3b589-175">Pro cílový kontejner Pokud zadaný kontejner neexistuje, bude vytvořena pro vás.</span><span class="sxs-lookup"><span data-stu-id="3b589-175">For the destination container, if the specified container doesn't exist, it will be created for you.</span></span> 

<span data-ttu-id="3b589-176">Pokud chcete použít AzCopy, otevřete příkazový řádek na místním počítači a přejděte do složky, kde je nainstalován nástroj AzCopy.</span><span class="sxs-lookup"><span data-stu-id="3b589-176">To use AzCopy, open a command prompt on your local machine and navigate to the folder where AzCopy is installed.</span></span> <span data-ttu-id="3b589-177">Je podobná *C:\Program Files (x86) \Microsoft SDKs\Azure\AzCopy*.</span><span class="sxs-lookup"><span data-stu-id="3b589-177">It will be similar to *C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy*.</span></span> 

<span data-ttu-id="3b589-178">Zkopírujte všechny soubory v rámci kontejneru, můžete použít **/S** přepínače.</span><span class="sxs-lookup"><span data-stu-id="3b589-178">To copy all of the files within a container, you use the **/S** switch.</span></span> <span data-ttu-id="3b589-179">Tímto lze kopírovat virtuální pevný disk operačního systému a všech datových disků, pokud jsou ve stejném kontejneru.</span><span class="sxs-lookup"><span data-stu-id="3b589-179">This can be used to copy the OS VHD and all of the data disks if they are in the same container.</span></span> <span data-ttu-id="3b589-180">Tento příklad ukazuje, jak zkopírovat všechny soubory v kontejneru **mysourcecontainer** v účtu úložiště **mysourcestorageaccount** ke kontejneru **mydestinationcontainer**v **mydestinationstorageaccount** účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="3b589-180">This example shows how to copy all of the files in the container **mysourcecontainer** in storage account **mysourcestorageaccount** to the container **mydestinationcontainer** in the **mydestinationstorageaccount** storage account.</span></span> <span data-ttu-id="3b589-181">Nahraďte názvy účtů úložiště a kontejnery vlastními.</span><span class="sxs-lookup"><span data-stu-id="3b589-181">Replace the names of the storage accounts and containers with your own.</span></span> <span data-ttu-id="3b589-182">Nahraďte `<sourceStorageAccountKey1>` a `<destinationStorageAccountKey1>` s vlastními klíči.</span><span class="sxs-lookup"><span data-stu-id="3b589-182">Replace `<sourceStorageAccountKey1>` and `<destinationStorageAccountKey1>` with your own keys.</span></span>

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
    /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
    /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

<span data-ttu-id="3b589-183">Pokud chcete zkopírovat konkrétní virtuální pevný disk v kontejneru s více soubory, můžete také zadat název souboru pomocí přepínače /Pattern.</span><span class="sxs-lookup"><span data-stu-id="3b589-183">If you only want to copy a specific VHD in a container with multiple files, you can also specify the file name using the /Pattern switch.</span></span> <span data-ttu-id="3b589-184">V tomto příkladu pouze soubor s názvem **myFileName.vhd** budou zkopírovány.</span><span class="sxs-lookup"><span data-stu-id="3b589-184">In this example, only the file named **myFileName.vhd** will be copied.</span></span>

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
  /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
  /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
  /Pattern:myFileName.vhd
```


<span data-ttu-id="3b589-185">Po dokončení, zobrazí se zpráva, která vypadá podobně jako:</span><span class="sxs-lookup"><span data-stu-id="3b589-185">When it is finished, you will get a message that looks something like:</span></span>

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

### <a name="troubleshooting"></a><span data-ttu-id="3b589-186">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="3b589-186">Troubleshooting</span></span>
* <span data-ttu-id="3b589-187">Při použití nástroje AZCopy, pokud se zobrazí chyba "Serveru se nepodařilo ověřit žádost", zajistěte, aby hodnotu hlavičky autorizace je vytvořen. správně včetně podpis.</span><span class="sxs-lookup"><span data-stu-id="3b589-187">When you use AZCopy, if you see the error "Server failed to authenticate the request", make sure the value of the Authorization header is formed correctly including the signature.</span></span> <span data-ttu-id="3b589-188">Pokud používáte 2 klíč nebo klíč sekundární úložiště, zkuste použít primární nebo 1. úložiště klíč.</span><span class="sxs-lookup"><span data-stu-id="3b589-188">If you are using Key 2 or the secondary storage key, try using the primary or 1st storage key.</span></span>

## <a name="create-the-new-vm"></a><span data-ttu-id="3b589-189">Vytvoření nového virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="3b589-189">Create the new VM</span></span> 

<span data-ttu-id="3b589-190">Budete muset vytvořit sítě a další prostředky virtuálních počítačů, které má být používána nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3b589-190">You need to create networking and other VM resources to be used by the new VM.</span></span>

### <a name="create-the-subnet-and-vnet"></a><span data-ttu-id="3b589-191">Vytvoření podsítě a sítě vNet</span><span class="sxs-lookup"><span data-stu-id="3b589-191">Create the subNet and vNet</span></span>

<span data-ttu-id="3b589-192">Vytvořit virtuální síť a podsíť [virtuální sítě](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3b589-192">Create the vNet and subNet of the [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="3b589-193">Vytvořte podsíť.</span><span class="sxs-lookup"><span data-stu-id="3b589-193">Create the subNet.</span></span> <span data-ttu-id="3b589-194">Tento příklad vytvoří podsíť s názvem **mySubNet**, ve skupině prostředků **myResourceGroup**a nastaví předpona adresy podsítě **10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="3b589-194">This example creates a subnet named **mySubNet**, in the resource group **myResourceGroup**, and sets the subnet address prefix to **10.0.0.0/24**.</span></span>
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="3b589-195">Vytvořte síť vNet.</span><span class="sxs-lookup"><span data-stu-id="3b589-195">Create the vNet.</span></span> <span data-ttu-id="3b589-196">Tento příklad nastaví název virtuální sítě, který se má **myVnetName**, umístění, do **západní USA**a předponu adresy virtuální sítě, abyste **10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="3b589-196">This example sets the virtual network name to be **myVnetName**, the location to **West US**, and the address prefix for the virtual network to **10.0.0.0/16**.</span></span> 
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-nic"></a><span data-ttu-id="3b589-197">Vytvoření veřejné IP adresy a síťové karty</span><span class="sxs-lookup"><span data-stu-id="3b589-197">Create a public IP address and NIC</span></span>
<span data-ttu-id="3b589-198">Pokud chcete povolit komunikaci s virtuálním počítačem ve virtuální síti, budete potřebovat [veřejnou adresu IP](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) a síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="3b589-198">To enable communication with the virtual machine in the virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="3b589-199">Vytvoření veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="3b589-199">Create the public IP.</span></span> <span data-ttu-id="3b589-200">V tomto příkladu je název veřejné IP adresy nastavena **myIP**.</span><span class="sxs-lookup"><span data-stu-id="3b589-200">In this example, the public IP address name is set to **myIP**.</span></span>
   
    ```powershell
    $ipName = "myIP"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="3b589-201">Vytvořit síťovou kartu.</span><span class="sxs-lookup"><span data-stu-id="3b589-201">Create the NIC.</span></span> <span data-ttu-id="3b589-202">V tomto příkladu je síťový adaptér je nastavit název **myNicName**.</span><span class="sxs-lookup"><span data-stu-id="3b589-202">In this example, the NIC name is set to **myNicName**.</span></span>
   
    ```powershell
    $nicName = "myNicName"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-the-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="3b589-203">Vytvořte skupinu zabezpečení sítě a pravidlo protokolu RDP</span><span class="sxs-lookup"><span data-stu-id="3b589-203">Create the network security group and an RDP rule</span></span>
<span data-ttu-id="3b589-204">Abyste mohli přihlásit k virtuálnímu počítači pomocí protokolu RDP, musíte mít pravidlo zabezpečení, která umožňuje přístup RDP na portu 3389.</span><span class="sxs-lookup"><span data-stu-id="3b589-204">To be able to log in to your VM using RDP, you need to have an security rule that allows RDP access on port 3389.</span></span> <span data-ttu-id="3b589-205">Protože byl vytvořen virtuální pevný disk pro nový virtuální počítač ze stávajícího specializované virtuálního počítače, po vytvoření virtuálního počítače je můžete použít existující účet ze zdrojového virtuálního počítače, který měl oprávnění k přihlášení pomocí protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="3b589-205">Because the VHD for the new VM was created from an existing specialized VM, after the VM is created you can use an existing account from the source virtual machine that had permission to log on using RDP.</span></span>
<span data-ttu-id="3b589-206">Tento příklad nastaví název skupiny NSG na **myNsg** a název pravidla protokolu RDP na **myRdpRule**.</span><span class="sxs-lookup"><span data-stu-id="3b589-206">This example sets the NSG name to **myNsg** and the RDP rule name to **myRdpRule**.</span></span>

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

<span data-ttu-id="3b589-207">Další informace o koncových bodů a pravidla NSG najdete v tématu [otevřít porty pro virtuální počítač v Azure pomocí prostředí PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3b589-207">For more information about endpoints and NSG rules, see [Opening ports to a VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

### <a name="set-the-vm-name-and-size"></a><span data-ttu-id="3b589-208">Nastavte název virtuálního počítače a velikosti</span><span class="sxs-lookup"><span data-stu-id="3b589-208">Set the VM name and size</span></span>

<span data-ttu-id="3b589-209">Tento příklad nastaví název virtuálního počítače na "Můjvp" a "Standard_A2" na velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3b589-209">This example sets the VM name to "myVM" and the VM size to "Standard_A2".</span></span>
```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-the-nic"></a><span data-ttu-id="3b589-210">Přidání síťového adaptéru</span><span class="sxs-lookup"><span data-stu-id="3b589-210">Add the NIC</span></span>
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    
    
### <a name="configure-the-os-disk"></a><span data-ttu-id="3b589-211">Konfigurace disku operačního systému</span><span class="sxs-lookup"><span data-stu-id="3b589-211">Configure the OS disk</span></span>

1. <span data-ttu-id="3b589-212">Nastavte identifikátor URI pro disk VHD, který jste nahráli nebo zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="3b589-212">Set the URI for the VHD that you uploaded or copied.</span></span> <span data-ttu-id="3b589-213">V tomto příkladu soubor virtuálního pevného disku s názvem **myOsDisk.vhd** je uložen v účtu úložiště s názvem **Můj_účet_úložiště** v kontejneru nazvaném **Můj_kontejner**.</span><span class="sxs-lookup"><span data-stu-id="3b589-213">In this example, the VHD file named **myOsDisk.vhd** is kept in a storage account named **myStorageAccount** in a container named **myContainer**.</span></span>

    ```powershell
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    ```
2. <span data-ttu-id="3b589-214">Přidejte disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="3b589-214">Add the OS disk.</span></span> <span data-ttu-id="3b589-215">V tomto příkladu když je vytvořen disk operačního systému, je termín "osDisk" appened na název virtuálního počítače. Chcete-li vytvořit název disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="3b589-215">In this example, when the OS disk is created, the term "osDisk" is appened to the VM name to create the OS disk name.</span></span> <span data-ttu-id="3b589-216">Tento příklad také určuje, že tento virtuální pevný disk systému Windows by měl být připojen virtuální počítač jako disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="3b589-216">This example also specifies that this Windows-based VHD should be attached to the VM as the OS disk.</span></span>
    
    ```powershell
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
    ```

<span data-ttu-id="3b589-217">Volitelné: Pokud máte datových disků, které musí být připojené k virtuálnímu počítači, přidejte datových disků pomocí adresy URL dat virtuálních pevných disků a odpovídající logické jednotky (LUN).</span><span class="sxs-lookup"><span data-stu-id="3b589-217">Optional: If you have data disks that need to be attached to the VM, add the data disks by using the URLs of data VHDs and the appropriate Logical Unit Number (Lun).</span></span>

```powershell
$dataDiskName = $vmName + "dataDisk"
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 1 -CreateOption attach
```

<span data-ttu-id="3b589-218">Pokud používáte účet úložiště, data a adresy URL disku operačního systému vypadat přibližně takto: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`.</span><span class="sxs-lookup"><span data-stu-id="3b589-218">When using a storage account, the data and operating system disk URLs look something like this: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`.</span></span> <span data-ttu-id="3b589-219">To můžete najít na portálu procházení, aby cílový kontejner úložiště, klepnutím na operačního systému nebo data virtuálního pevného disku, který jste zkopírovali, a pak kopírování obsah adresy URL.</span><span class="sxs-lookup"><span data-stu-id="3b589-219">You can find this on the portal by browsing to the target storage container, clicking the operating system or data VHD that was copied, and then copying the contents of the URL.</span></span>


### <a name="complete-the-vm"></a><span data-ttu-id="3b589-220">Dokončení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="3b589-220">Complete the VM</span></span> 

<span data-ttu-id="3b589-221">Vytvoření virtuálního počítače pomocí konfigurace, které jsme právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="3b589-221">Create the VM using the configurations that we just created.</span></span>

```powershell
#Create the new VM
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

<span data-ttu-id="3b589-222">Pokud tento příkaz byl úspěšný, zobrazí se výstup takto:</span><span class="sxs-lookup"><span data-stu-id="3b589-222">If this command was successful, you'll see output like this:</span></span>

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-the-vm-was-created"></a><span data-ttu-id="3b589-223">Zkontrolujte, zda byl vytvořen virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="3b589-223">Verify that the VM was created</span></span>
<span data-ttu-id="3b589-224">Měli byste vidět nově vytvořený virtuální počítač buď v [portál Azure](https://portal.azure.com)v části **Procházet** > **virtuální počítače**, nebo pomocí následujících příkazů prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="3b589-224">You should see the newly created VM either in the [Azure portal](https://portal.azure.com), under **Browse** > **Virtual machines**, or by using the following PowerShell commands:</span></span>

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $rgName
$vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="3b589-225">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3b589-225">Next steps</span></span>
<span data-ttu-id="3b589-226">K přihlášení do nového virtuálního počítače, přejděte do virtuálního počítače v [portál](https://portal.azure.com), klikněte na tlačítko **Connect**a otevřete soubor Remote Desktop RDP.</span><span class="sxs-lookup"><span data-stu-id="3b589-226">To sign in to your new virtual machine, browse to the VM in the [portal](https://portal.azure.com), click **Connect**, and open the Remote Desktop RDP file.</span></span> <span data-ttu-id="3b589-227">Pomocí přihlašovacích údajů účtu původní virtuálního počítače pro přihlášení do nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3b589-227">Use the account credentials of your original virtual machine to sign in to your new virtual machine.</span></span> <span data-ttu-id="3b589-228">Další informace najdete v tématu [jak se připojit a přihlásit se na virtuálním počítači Azure s Windows](connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="3b589-228">For more information, see [How to connect and log on to an Azure virtual machine running Windows](connect-logon.md).</span></span>

