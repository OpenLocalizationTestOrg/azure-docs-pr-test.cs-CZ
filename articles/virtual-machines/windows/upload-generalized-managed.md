---
title: "Vytvoření spravovaných virtuálních počítačů Azure z virtuální pevný disk zobecněný místní | Microsoft Docs"
description: "Nahrajte zobecněný virtuální pevný disk do Azure a použít ho k vytvoření nové virtuální počítače, v modelu nasazení Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: cynthn
ms.openlocfilehash: d802ba16ecb4e32e2adb7be3a8e99c72a1625841
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="upload-a-generalized-vhd-and-use-it-to-create-new-vms-in-azure"></a><span data-ttu-id="e8312-103">Nahrát zobecněný virtuální pevný disk a použít ho k vytvoření nové virtuální počítače v Azure</span><span class="sxs-lookup"><span data-stu-id="e8312-103">Upload a generalized VHD and use it to create new VMs in Azure</span></span>

<span data-ttu-id="e8312-104">Toto téma vás provede pomocí prostředí PowerShell nahrajte virtuální pevný disk zobecněný virtuální počítač do Azure, vytvořte bitovou kopii z disku VHD a vytvoření nového virtuálního počítače z této bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="e8312-104">This topic walks you through using PowerShell to upload a VHD of a generalized VM to Azure, create an image from the VHD and create a new VM from that image.</span></span> <span data-ttu-id="e8312-105">Můžete nahrát virtuální pevný disk exportovat z nástroj virtualizace na místní nebo z jiného cloudu.</span><span class="sxs-lookup"><span data-stu-id="e8312-105">You can upload a VHD exported from an on-premises virtualization tool or from another cloud.</span></span> <span data-ttu-id="e8312-106">Pomocí [spravované disky](managed-disks-overview.md) pro nový virtuální počítač zjednodušuje správu virtuálních počítačů a poskytuje lepší dostupnost při umístění virtuálního počítače do skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="e8312-106">Using [Managed Disks](managed-disks-overview.md) for the new VM simplifies the VM managment and provides better availability when the VM is placed in an availability set.</span></span> 

<span data-ttu-id="e8312-107">Pokud chcete použít ukázkový skript, přečtěte si téma [ukázkový skript nahrání virtuálního pevného disku do Azure a vytvoření nového virtuálního počítače](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md)</span><span class="sxs-lookup"><span data-stu-id="e8312-107">If you want to use a sample script, see [Sample script to upload a VHD to Azure and create a new VM](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md)</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="e8312-108">Než začnete</span><span class="sxs-lookup"><span data-stu-id="e8312-108">Before you begin</span></span>

- <span data-ttu-id="e8312-109">Před nahráním jakéhokoli virtuálního pevného disku do Azure, postupujte podle [Příprava Windows VHD nebo VHDX, který chcete nahrát do Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="e8312-109">Before uploading any VHD to Azure, you should follow [Prepare a Windows VHD or VHDX to upload to Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>
- <span data-ttu-id="e8312-110">Zkontrolujte [plánování migrace na spravované disky](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) před zahájením migrace na [spravované disky](managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e8312-110">Review [Plan for the migration to Managed Disks](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) before starting your migration to [Managed Disks](managed-disks-overview.md).</span></span>
- <span data-ttu-id="e8312-111">Ujistěte se, že máte nejnovější verzi modulu AzureRM.Compute prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e8312-111">Make sure that you have the latest version of the AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="e8312-112">Spusťte následující příkaz k její instalaci.</span><span class="sxs-lookup"><span data-stu-id="e8312-112">Run the following command to install it.</span></span>

    ```powershell
    Install-Module AzureRM.Compute -RequiredVersion 2.6.0
    ```
    <span data-ttu-id="e8312-113">Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e8312-113">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="generalize-the-windows-vm-using-sysprep"></a><span data-ttu-id="e8312-114">Generalize virtuální počítač s Windows pomocí nástroje Sysprep</span><span class="sxs-lookup"><span data-stu-id="e8312-114">Generalize the Windows VM using Sysprep</span></span>

<span data-ttu-id="e8312-115">Nástroj Sysprep odstraní všechny vaše osobní informace o účtu, mimo jiné a připraví počítač, který se má použít jako obrázek.</span><span class="sxs-lookup"><span data-stu-id="e8312-115">Sysprep removes all your personal account information, among other things, and prepares the machine to be used as an image.</span></span> <span data-ttu-id="e8312-116">Podrobnosti o nástroji Sysprep najdete v tématu [postup použití nástroje Sysprep: Úvod](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="e8312-116">For details about Sysprep, see [How to Use Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>

<span data-ttu-id="e8312-117">Ujistěte se, že role serveru spuštěná na tomto počítači jsou podporovány nástrojem Sysprep.</span><span class="sxs-lookup"><span data-stu-id="e8312-117">Make sure the server roles running on the machine are supported by Sysprep.</span></span> <span data-ttu-id="e8312-118">Další informace najdete v tématu [podpora nástroje Sysprep pro role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span><span class="sxs-lookup"><span data-stu-id="e8312-118">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e8312-119">Pokud používáte nástroj Sysprep před nahráním svůj disk VHD do Azure poprvé, ujistěte se, máte [připravit virtuální počítač](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) před spuštěním nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="e8312-119">If you are running Sysprep before uploading your VHD to Azure for the first time, make sure you have [prepared your VM](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before running Sysprep.</span></span> 
> 
> 

1. <span data-ttu-id="e8312-120">Přihlaste se k virtuálnímu počítači Windows.</span><span class="sxs-lookup"><span data-stu-id="e8312-120">Sign in to the Windows virtual machine.</span></span>
2. <span data-ttu-id="e8312-121">Otevřete okno příkazového řádku jako správce.</span><span class="sxs-lookup"><span data-stu-id="e8312-121">Open the Command Prompt window as an administrator.</span></span> <span data-ttu-id="e8312-122">Změňte adresář na **%windir%\system32\sysprep**a poté spusťte `sysprep.exe`.</span><span class="sxs-lookup"><span data-stu-id="e8312-122">Change the directory to **%windir%\system32\sysprep**, and then run `sysprep.exe`.</span></span>
3. <span data-ttu-id="e8312-123">V **nástroj pro přípravu systému** dialogové okno, vyberte **prostředí Out-of-Box zadejte systému (při prvním zapnutí)**a ujistěte se, že **generalizace** je zaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="e8312-123">In the **System Preparation Tool** dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that the **Generalize** check box is selected.</span></span>
4. <span data-ttu-id="e8312-124">V **možnosti vypnutí**, vyberte **vypnutí**.</span><span class="sxs-lookup"><span data-stu-id="e8312-124">In **Shutdown Options**, select **Shutdown**.</span></span>
5. <span data-ttu-id="e8312-125">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="e8312-125">Click **OK**.</span></span>
   
    ![Spusťte nástroj Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. <span data-ttu-id="e8312-127">Po dokončení nástroj Sysprep vypne virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e8312-127">When Sysprep completes, it shuts down the virtual machine.</span></span> <span data-ttu-id="e8312-128">Virtuální počítač nerestartuje.</span><span class="sxs-lookup"><span data-stu-id="e8312-128">Do not restart the VM.</span></span>



## <a name="log-in-to-azure"></a><span data-ttu-id="e8312-129">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="e8312-129">Log in to Azure</span></span>
<span data-ttu-id="e8312-130">Pokud ještě nemáte prostředí PowerShell verze 1.4 nebo vyšší nainstalovaná, přečtěte si [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e8312-130">If you don't already have PowerShell version 1.4 or above installed, read [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

1. <span data-ttu-id="e8312-131">Otevřete prostředí Azure PowerShell a přihlaste se k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="e8312-131">Open Azure PowerShell and sign in to your Azure account.</span></span> <span data-ttu-id="e8312-132">Automaticky otevírané okno otevře zadat přihlašovací údaje účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="e8312-132">A pop-up window opens for you to enter your Azure account credentials.</span></span>
   
    ```powershell
    Login-AzureRmAccount
    ```
2. <span data-ttu-id="e8312-133">Pořiďte si předplatné ID pro dostupných předplatných.</span><span class="sxs-lookup"><span data-stu-id="e8312-133">Get the subscription IDs for your available subscriptions.</span></span>
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. <span data-ttu-id="e8312-134">Nastavit správné předplatné pomocí ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="e8312-134">Set the correct subscription using the subscription ID.</span></span> <span data-ttu-id="e8312-135">Nahraďte  *<subscriptionID>*  s ID správné předplatné.</span><span class="sxs-lookup"><span data-stu-id="e8312-135">Replace *<subscriptionID>* with the ID of the correct subscription.</span></span>
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="get-the-storage-account"></a><span data-ttu-id="e8312-136">Získat účet úložiště</span><span class="sxs-lookup"><span data-stu-id="e8312-136">Get the storage account</span></span>
<span data-ttu-id="e8312-137">Potřebujete účet úložiště v Azure k ukládání nahrané image virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e8312-137">You need a storage account in Azure to store the uploaded VM image.</span></span> <span data-ttu-id="e8312-138">Můžete použít existující účet úložiště, nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="e8312-138">You can either use an existing storage account or create a new one.</span></span> 

<span data-ttu-id="e8312-139">Pokud budete používat virtuální pevný disk pro vytvoření spravovaného disku pro virtuální počítač, umístění účtu úložiště musí být stejné umístění, kde bude vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e8312-139">If you will be using the VHD to create a managed disk for a VM, the storage account location must be same the location where you will be creating the VM.</span></span>

<span data-ttu-id="e8312-140">Chcete-li zobrazit účty úložiště k dispozici, zadejte:</span><span class="sxs-lookup"><span data-stu-id="e8312-140">To show the available storage accounts, type:</span></span>

```powershell
Get-AzureRmStorageAccount
```

<span data-ttu-id="e8312-141">Pokud chcete použít existující účet úložiště, pokračujte [nahrajte image virtuálního počítače](#upload-the-vm-vhd-to-your-storage-account) části.</span><span class="sxs-lookup"><span data-stu-id="e8312-141">If you want to use an existing storage account, proceed to the [Upload the VM image](#upload-the-vm-vhd-to-your-storage-account) section.</span></span>

<span data-ttu-id="e8312-142">Pokud potřebujete vytvořit účet úložiště, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e8312-142">If you need to create a storage account, follow these steps:</span></span>

1. <span data-ttu-id="e8312-143">Je třeba na název skupiny prostředků, kde by měl být vytvořen účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="e8312-143">You need the name of the resource group where the storage account should be created.</span></span> <span data-ttu-id="e8312-144">Chcete-li zjistit všechny skupiny prostředků, které jsou v rámci vašeho předplatného, zadejte:</span><span class="sxs-lookup"><span data-stu-id="e8312-144">To find out all the resource groups that are in your subscription, type:</span></span>
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    <span data-ttu-id="e8312-145">Chcete-li vytvořit skupinu prostředků s názvem **myResourceGroup** v **východní USA** oblast, zadejte:</span><span class="sxs-lookup"><span data-stu-id="e8312-145">To create a resource group named **myResourceGroup** in the **East US** region, type:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "East US"
    ```

2. <span data-ttu-id="e8312-146">Vytvořit účet úložiště s názvem **můj_účet_úložiště** v této skupině prostředků s použitím [AzureRmStorageAccount nový](/powershell/module/azurerm.storage/new-azurermstorageaccount) rutiny:</span><span class="sxs-lookup"><span data-stu-id="e8312-146">Create a storage account named **mystorageaccount** in this resource group by using the [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span></span>
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "East US"`
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
   
    <span data-ttu-id="e8312-147">Platné hodnoty - SkuName jsou:</span><span class="sxs-lookup"><span data-stu-id="e8312-147">Valid values for -SkuName are:</span></span>
   
   * <span data-ttu-id="e8312-148">**Standard_LRS** -místně redundantní úložiště.</span><span class="sxs-lookup"><span data-stu-id="e8312-148">**Standard_LRS** - Locally redundant storage.</span></span> 
   * <span data-ttu-id="e8312-149">**Standard_ZRS** -zónu redundantní úložiště.</span><span class="sxs-lookup"><span data-stu-id="e8312-149">**Standard_ZRS** - Zone redundant storage.</span></span>
   * <span data-ttu-id="e8312-150">**Standard_GRS** -geograficky redundantní úložiště.</span><span class="sxs-lookup"><span data-stu-id="e8312-150">**Standard_GRS** - Geo redundant storage.</span></span> 
   * <span data-ttu-id="e8312-151">**Standard_RAGRS** -geograficky redundantní úložiště s přístupem pro čtení.</span><span class="sxs-lookup"><span data-stu-id="e8312-151">**Standard_RAGRS** - Read access geo redundant storage.</span></span> 
   * <span data-ttu-id="e8312-152">**Premium_LRS** -místně redundantního úložiště Premium.</span><span class="sxs-lookup"><span data-stu-id="e8312-152">**Premium_LRS** - Premium locally redundant storage.</span></span> 

## <a name="upload-the-vhd-to-your-storage-account"></a><span data-ttu-id="e8312-153">Nahrání virtuálního pevného disku do účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="e8312-153">Upload the VHD to your storage account</span></span>

<span data-ttu-id="e8312-154">Použití [přidat AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx) rutiny odeslání disku VHD do kontejneru v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e8312-154">Use the [Add-AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx) cmdlet to upload the VHD to a container in your storage account.</span></span> <span data-ttu-id="e8312-155">Tento příklad nahrávání souboru *myVHD.vhd* z *"C:\Users\Public\Documents\Virtual pevné disky\"*  na účet úložiště s názvem *můj_účet_úložiště* v *myResourceGroup* skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="e8312-155">This example uploads the file *myVHD.vhd* from *"C:\Users\Public\Documents\Virtual hard disks\"* to a storage account named *mystorageaccount* in the *myResourceGroup* resource group.</span></span> <span data-ttu-id="e8312-156">Soubor se umístí do kontejner s názvem *můj_kontejner* a nový název souboru bude *myUploadedVHD.vhd*.</span><span class="sxs-lookup"><span data-stu-id="e8312-156">The file will be placed into the container named *mycontainer* and the new file name will be *myUploadedVHD.vhd*.</span></span>

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


<span data-ttu-id="e8312-157">Pokud bylo úspěšné, můžete získat odpovědi, která vypadá podobně jako tento:</span><span class="sxs-lookup"><span data-stu-id="e8312-157">If successful, you get a response that looks similar to this:</span></span>

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

<span data-ttu-id="e8312-158">V závislosti na připojení k síti a velikost souboru virtuálního pevného disku tento příkaz může trvat nějakou dobu pro dokončení</span><span class="sxs-lookup"><span data-stu-id="e8312-158">Depending on your network connection and the size of your VHD file, this command may take a while to complete</span></span>

<span data-ttu-id="e8312-159">Uložit **identifikátor URI pro cíl** cestu pro pozdější použití, pokud se chystáte vytvořit spravovaných disků nebo nového virtuálního počítače pomocí nahraný virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="e8312-159">Save the **Destination URI** path to use later if you are going to create a managed disk or a new VM using the uploaded VHD.</span></span>

### <a name="other-options-for-uploading-a-vhd"></a><span data-ttu-id="e8312-160">Další možnosti pro odesílání virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="e8312-160">Other options for uploading a VHD</span></span>
 
 
<span data-ttu-id="e8312-161">Můžete také nahrát virtuální pevný disk na váš účet úložiště pomocí jedné z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="e8312-161">You can also upload a VHD to your storage account using one of the following:</span></span>

- [<span data-ttu-id="e8312-162">AzCopy</span><span class="sxs-lookup"><span data-stu-id="e8312-162">AzCopy</span></span>](http://aka.ms/downloadazcopy)
- [<span data-ttu-id="e8312-163">Objekt Blob služby Azure Storage kopie rozhraní API</span><span class="sxs-lookup"><span data-stu-id="e8312-163">Azure Storage Copy Blob API</span></span>](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [<span data-ttu-id="e8312-164">Objektů BLOB Azure Storage Explorer odesílání</span><span class="sxs-lookup"><span data-stu-id="e8312-164">Azure Storage Explorer Uploading Blobs</span></span>](https://azurestorageexplorer.codeplex.com/)
- [<span data-ttu-id="e8312-165">Referenční dokumentace rozhraní API úložiště importu/exportu služby REST</span><span class="sxs-lookup"><span data-stu-id="e8312-165">Storage Import/Export Service REST API Reference</span></span>](https://msdn.microsoft.com/library/dn529096.aspx)
-   <span data-ttu-id="e8312-166">Doporučujeme používat službu Import/Export, pokud je předpokládaná doba odesílání doba je delší než 7 dní.</span><span class="sxs-lookup"><span data-stu-id="e8312-166">We recommend using Import/Export Service if estimated uploading time is longer than 7 days.</span></span> <span data-ttu-id="e8312-167">Můžete použít [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) k zjištění přibližné hodnoty času z jednotky velikost a přenášet data.</span><span class="sxs-lookup"><span data-stu-id="e8312-167">You can use [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) to estimate the time from data size and transfer unit.</span></span> 
    <span data-ttu-id="e8312-168">Import a Export lze použít pro kopírování na standardní účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="e8312-168">Import/Export can be used to copy to a standard storage account.</span></span> <span data-ttu-id="e8312-169">Musíte zkopírovat z úložiště standard storage do prémiový účet úložiště pomocí nástroje, například AzCopy.</span><span class="sxs-lookup"><span data-stu-id="e8312-169">You will need to copy from standard storage to premium storage account using a tool like AzCopy.</span></span>


## <a name="create-a-managed-image-from-the-uploaded-vhd"></a><span data-ttu-id="e8312-170">Vytvoření bitové kopie spravované z nahraný virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="e8312-170">Create a managed image from the uploaded VHD</span></span> 

<span data-ttu-id="e8312-171">Vytvoření spravované image pomocí vaší zobecněný virtuální pevný disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="e8312-171">Create a managed image using your generalized OS VHD.</span></span> <span data-ttu-id="e8312-172">Nahraďte hodnoty svými vlastními informacemi.</span><span class="sxs-lookup"><span data-stu-id="e8312-172">Replace the values with your own information.</span></span>


1.  <span data-ttu-id="e8312-173">Nastavte nejprve, běžné parametry:</span><span class="sxs-lookup"><span data-stu-id="e8312-173">First, set the common parameters:</span></span>

    ```powershell
    $vmName = "myVM"
    $computerName = "myComputer"
    $vmSize = "Standard_DS1_v2"
    $location = "East US" 
    $imageName = "yourImageName"
    ```

4.  <span data-ttu-id="e8312-174">Vytvořte bitovou kopii pomocí vaší zobecněný virtuální pevný disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="e8312-174">Create the image using your generalized OS VHD.</span></span>

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $urlOfUploadedImageVhd
    $image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```

## <a name="create-a-virtual-network"></a><span data-ttu-id="e8312-175">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="e8312-175">Create a virtual network</span></span>
<span data-ttu-id="e8312-176">Vytvořit virtuální síť a podsíť [virtuální sítě](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e8312-176">Create the vNet and subnet of the [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="e8312-177">Vytvořte podsíť.</span><span class="sxs-lookup"><span data-stu-id="e8312-177">Create the subnet.</span></span> <span data-ttu-id="e8312-178">Tento příklad vytvoří podsíť s názvem *mySubnet* s předponou adresy z *10.0.0.0/24*.</span><span class="sxs-lookup"><span data-stu-id="e8312-178">This example creates a subnet named *mySubnet* with the address prefix of *10.0.0.0/24*.</span></span>  
   
    ```powershell
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="e8312-179">Vytvořte virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="e8312-179">Create the virtual network.</span></span> <span data-ttu-id="e8312-180">Tento příklad vytvoří virtuální síť s názvem *myVnet* s předponou adresy z *10.0.0.0/16*.</span><span class="sxs-lookup"><span data-stu-id="e8312-180">This example creates a virtual network named *myVnet* with the address prefix of *10.0.0.0/16*.</span></span>  
   
    ```powershell
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-network-interface"></a><span data-ttu-id="e8312-181">Vytvoření veřejné IP adresy a síťového rozhraní</span><span class="sxs-lookup"><span data-stu-id="e8312-181">Create a public IP address and network interface</span></span>

<span data-ttu-id="e8312-182">Pokud chcete povolit komunikaci s virtuálním počítačem ve virtuální síti, budete potřebovat [veřejnou adresu IP](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) a síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e8312-182">To enable communication with the virtual machine in the virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="e8312-183">Vytvoření veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="e8312-183">Create a public IP address.</span></span> <span data-ttu-id="e8312-184">Tento příklad vytvoří veřejnou IP adresu s názvem *myPip*.</span><span class="sxs-lookup"><span data-stu-id="e8312-184">This example creates a public IP address named *myPip*.</span></span> 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="e8312-185">Vytvořit síťovou kartu.</span><span class="sxs-lookup"><span data-stu-id="e8312-185">Create the NIC.</span></span> <span data-ttu-id="e8312-186">Tento příklad vytvoří síťový adaptér s názvem **myNic**.</span><span class="sxs-lookup"><span data-stu-id="e8312-186">This example creates a NIC named **myNic**.</span></span> 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="e8312-187">Vytvořte skupinu zabezpečení sítě a pravidlo protokolu RDP</span><span class="sxs-lookup"><span data-stu-id="e8312-187">Create the network security group and an RDP rule</span></span>

<span data-ttu-id="e8312-188">Abyste mohli přihlásit k virtuálnímu počítači pomocí protokolu RDP, musíte mít pravidla zabezpečení sítě (NSG), který umožňuje přístup RDP na portu 3389.</span><span class="sxs-lookup"><span data-stu-id="e8312-188">To be able to log in to your VM using RDP, you need to have a network security rule (NSG) that allows RDP access on port 3389.</span></span> 

<span data-ttu-id="e8312-189">Tento příklad vytvoří skupinu NSG s názvem *myNsg* obsahující pravidlo názvem *myRdpRule* přes port 3389 umožňuje provoz protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="e8312-189">This example creates an NSG named *myNsg* that contains a rule called *myRdpRule* that allows RDP traffic over port 3389.</span></span> <span data-ttu-id="e8312-190">Další informace o skupinách Nsg najdete v tématu [otevřít porty pro virtuální počítač v Azure pomocí prostředí PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e8312-190">For more information about NSGs, see [Opening ports to a VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

```powershell
$nsgName = "myNsg"
$ruleName = "myRdpRule"
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-the-virtual-network"></a><span data-ttu-id="e8312-191">Vytvořte proměnnou pro virtuální síť</span><span class="sxs-lookup"><span data-stu-id="e8312-191">Create a variable for the virtual network</span></span>

<span data-ttu-id="e8312-192">Vytvořte proměnnou pro dokončené virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="e8312-192">Create a variable for the completed virtual network.</span></span> 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

## <a name="get-the-credentials-for-the-vm"></a><span data-ttu-id="e8312-193">Získání přihlašovacích údajů pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e8312-193">Get the credentials for the VM</span></span>

<span data-ttu-id="e8312-194">Následující rutiny otevře okno, kde bude zadejte nové uživatelské jméno a heslo pro použití jako účet místního správce pro vzdálený přístup k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="e8312-194">The following cmdlet will open a window where you will enter a new user name and password to use as the local administrator account for remotely accessing the VM.</span></span> 

```powershell
$cred = Get-Credential
```

## <a name="add-the-vm-name-and-size-to-the-vm-configuration"></a><span data-ttu-id="e8312-195">Přidejte název virtuálního počítače a velikost ke konfiguraci virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e8312-195">Add the VM name and size to the VM configuration.</span></span>

```powershell
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

## <a name="set-the-vm-image-as-source-image-for-the-new-vm"></a><span data-ttu-id="e8312-196">Nastavit image virtuálního počítače jako zdroj bitové kopie pro nový virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="e8312-196">Set the VM image as source image for the new VM</span></span>

<span data-ttu-id="e8312-197">Nastavte zdrojové bitové kopie pomocí ID spravované image virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e8312-197">Set the source image using the ID of the managed VM image.</span></span>

```powershell
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## <a name="set-the-os-configuration-and-add-the-nic"></a><span data-ttu-id="e8312-198">Nastavení konfigurace operačního systému a přidejte na síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="e8312-198">Set the OS configuration and add the NIC.</span></span>

<span data-ttu-id="e8312-199">Zadejte typ úložiště (PremiumLRS nebo StandardLRS) a velikost disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="e8312-199">Enter the storage type (PremiumLRS or StandardLRS) and the size of the OS disk.</span></span> <span data-ttu-id="e8312-200">Tento příklad nastaví typ účtu *PremiumLRS*, velikost disku na *128 GB* a ukládání do mezipaměti na disku ke *ReadWrite*.</span><span class="sxs-lookup"><span data-stu-id="e8312-200">This example sets the account type to *PremiumLRS*, the disk size to *128 GB* and disk caching to *ReadWrite*.</span></span>

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## <a name="create-the-vm"></a><span data-ttu-id="e8312-201">Vytvořte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e8312-201">Create the VM</span></span>

<span data-ttu-id="e8312-202">Vytvoření nového virtuálního počítače pomocí konfigurace uložené v **$vm** proměnné.</span><span class="sxs-lookup"><span data-stu-id="e8312-202">Create the new VM using the configuration stored in the **$vm** variable.</span></span>

```powershell
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

## <a name="verify-that-the-vm-was-created"></a><span data-ttu-id="e8312-203">Zkontrolujte, zda byl vytvořen virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="e8312-203">Verify that the VM was created</span></span>
<span data-ttu-id="e8312-204">Po dokončení byste měli vidět nově vytvořený virtuální počítač v [portál Azure](https://portal.azure.com) pod **Procházet** > **virtuální počítače**, nebo pomocí následujících příkazů prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="e8312-204">When complete, you should see the newly created VM in the [Azure portal](https://portal.azure.com) under **Browse** > **Virtual machines**, or by using the following PowerShell commands:</span></span>

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="e8312-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e8312-205">Next steps</span></span>

<span data-ttu-id="e8312-206">K přihlášení do nového virtuálního počítače, přejděte do virtuálního počítače v [portál](https://portal.azure.com), klikněte na tlačítko **Connect**a otevřete soubor Remote Desktop RDP.</span><span class="sxs-lookup"><span data-stu-id="e8312-206">To sign in to your new virtual machine, browse to the VM in the [portal](https://portal.azure.com), click **Connect**, and open the Remote Desktop RDP file.</span></span> <span data-ttu-id="e8312-207">Pomocí přihlašovacích údajů účtu původní virtuálního počítače pro přihlášení do nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e8312-207">Use the account credentials of your original virtual machine to sign in to your new virtual machine.</span></span> <span data-ttu-id="e8312-208">Další informace najdete v tématu [jak se připojit a přihlásit se na virtuálním počítači Azure s Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e8312-208">For more information, see [How to connect and log on to an Azure virtual machine running Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

