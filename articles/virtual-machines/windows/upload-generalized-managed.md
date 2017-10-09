---
title: "aaaCreate spravovaných virtuálních počítačů Azure z disku VHD zobecněný místní | Microsoft Docs"
description: "Nahrát zobecněný virtuální pevný disk tooAzure a použít ho toocreate nové virtuální počítače, v modelu nasazení Resource Manager hello."
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
ms.openlocfilehash: 2fd0c0eec922e6ca8af4e712c1bceb1f9466105c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-a-generalized-vhd-and-use-it-toocreate-new-vms-in-azure"></a><span data-ttu-id="4a143-103">Nahrát zobecněný virtuální pevný disk a použít ho toocreate nové virtuální počítače v Azure</span><span class="sxs-lookup"><span data-stu-id="4a143-103">Upload a generalized VHD and use it toocreate new VMs in Azure</span></span>

<span data-ttu-id="4a143-104">Toto téma vás provede pomocí prostředí PowerShell tooupload virtuální pevný disk zobecněný tooAzure virtuálního počítače, vytvořte bitovou kopii z hello virtuálního pevného disku a vytvoření nového virtuálního počítače z této bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="4a143-104">This topic walks you through using PowerShell tooupload a VHD of a generalized VM tooAzure, create an image from hello VHD and create a new VM from that image.</span></span> <span data-ttu-id="4a143-105">Můžete nahrát virtuální pevný disk exportovat z nástroj virtualizace na místní nebo z jiného cloudu.</span><span class="sxs-lookup"><span data-stu-id="4a143-105">You can upload a VHD exported from an on-premises virtualization tool or from another cloud.</span></span> <span data-ttu-id="4a143-106">Pomocí [spravované disky](managed-disks-overview.md) hello nový virtuální počítač zjednodušuje správu hello virtuálních počítačů a poskytuje lepší dostupnost hello virtuálních počítačů při umístění do nastavení dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="4a143-106">Using [Managed Disks](managed-disks-overview.md) for hello new VM simplifies hello VM managment and provides better availability when hello VM is placed in an availability set.</span></span> 

<span data-ttu-id="4a143-107">Pokud chcete toouse ukázkový skript, najdete v části [ukázkový skript tooupload tooAzure virtuální pevný disk a vytvořte nový virtuální počítač](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md)</span><span class="sxs-lookup"><span data-stu-id="4a143-107">If you want toouse a sample script, see [Sample script tooupload a VHD tooAzure and create a new VM](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md)</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="4a143-108">Než začnete</span><span class="sxs-lookup"><span data-stu-id="4a143-108">Before you begin</span></span>

- <span data-ttu-id="4a143-109">Před nahráním žádné tooAzure virtuálního pevného disku, postupujte podle [připravit tooAzure tooupload Windows VHD nebo VHDX](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="4a143-109">Before uploading any VHD tooAzure, you should follow [Prepare a Windows VHD or VHDX tooupload tooAzure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>
- <span data-ttu-id="4a143-110">Zkontrolujte [plánování migrace hello disky tooManaged](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) před zahájením migrace příliš[spravované disky](managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4a143-110">Review [Plan for hello migration tooManaged Disks](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) before starting your migration too[Managed Disks](managed-disks-overview.md).</span></span>
- <span data-ttu-id="4a143-111">Ujistěte se, že máte nejnovější verzi hello modulu AzureRM.Compute PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="4a143-111">Make sure that you have hello latest version of hello AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="4a143-112">Spustit hello následující příkaz tooinstall.</span><span class="sxs-lookup"><span data-stu-id="4a143-112">Run hello following command tooinstall it.</span></span>

    ```powershell
    Install-Module AzureRM.Compute -RequiredVersion 2.6.0
    ```
    <span data-ttu-id="4a143-113">Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4a143-113">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="generalize-hello-windows-vm-using-sysprep"></a><span data-ttu-id="4a143-114">Generalize hello virtuální počítač s Windows pomocí nástroje Sysprep</span><span class="sxs-lookup"><span data-stu-id="4a143-114">Generalize hello Windows VM using Sysprep</span></span>

<span data-ttu-id="4a143-115">Nástroj Sysprep odstraní všechny vaše osobní informace o účtu, mimo jiné a připraví toobe počítač hello použít jako obrázek.</span><span class="sxs-lookup"><span data-stu-id="4a143-115">Sysprep removes all your personal account information, among other things, and prepares hello machine toobe used as an image.</span></span> <span data-ttu-id="4a143-116">Podrobnosti o nástroji Sysprep najdete v tématu [jak tooUse nástroje Sysprep: Úvod](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="4a143-116">For details about Sysprep, see [How tooUse Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>

<span data-ttu-id="4a143-117">Ujistěte se, že role serveru hello spuštěné na počítači hello jsou podporovány nástrojem Sysprep.</span><span class="sxs-lookup"><span data-stu-id="4a143-117">Make sure hello server roles running on hello machine are supported by Sysprep.</span></span> <span data-ttu-id="4a143-118">Další informace najdete v tématu [podpora nástroje Sysprep pro role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span><span class="sxs-lookup"><span data-stu-id="4a143-118">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4a143-119">Pokud používáte nástroj Sysprep před nahráním vaší tooAzure virtuálního pevného disku pro hello poprvé, ujistěte se, máte [připravit virtuální počítač](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) před spuštěním nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="4a143-119">If you are running Sysprep before uploading your VHD tooAzure for hello first time, make sure you have [prepared your VM](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before running Sysprep.</span></span> 
> 
> 

1. <span data-ttu-id="4a143-120">Přihlaste se toohello virtuálního počítače Windows.</span><span class="sxs-lookup"><span data-stu-id="4a143-120">Sign in toohello Windows virtual machine.</span></span>
2. <span data-ttu-id="4a143-121">Otevřete okno příkazového řádku hello jako správce.</span><span class="sxs-lookup"><span data-stu-id="4a143-121">Open hello Command Prompt window as an administrator.</span></span> <span data-ttu-id="4a143-122">Změnit adresář, hello příliš**%windir%\system32\sysprep**a poté spusťte `sysprep.exe`.</span><span class="sxs-lookup"><span data-stu-id="4a143-122">Change hello directory too**%windir%\system32\sysprep**, and then run `sysprep.exe`.</span></span>
3. <span data-ttu-id="4a143-123">V hello **nástroj pro přípravu systému** dialogové okno, vyberte **prostředí Out-of-Box zadejte systému (při prvním zapnutí)**a ujistěte se, že hello **generalizace** je zaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="4a143-123">In hello **System Preparation Tool** dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that hello **Generalize** check box is selected.</span></span>
4. <span data-ttu-id="4a143-124">V **možnosti vypnutí**, vyberte **vypnutí**.</span><span class="sxs-lookup"><span data-stu-id="4a143-124">In **Shutdown Options**, select **Shutdown**.</span></span>
5. <span data-ttu-id="4a143-125">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="4a143-125">Click **OK**.</span></span>
   
    ![Spusťte nástroj Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. <span data-ttu-id="4a143-127">Po dokončení nástroj Sysprep vypne hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4a143-127">When Sysprep completes, it shuts down hello virtual machine.</span></span> <span data-ttu-id="4a143-128">Nerestartovat hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4a143-128">Do not restart hello VM.</span></span>



## <a name="log-in-tooazure"></a><span data-ttu-id="4a143-129">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="4a143-129">Log in tooAzure</span></span>
<span data-ttu-id="4a143-130">Pokud ještě nemáte prostředí PowerShell verze 1.4 nebo vyšší nainstalovaná, přečtěte si [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4a143-130">If you don't already have PowerShell version 1.4 or above installed, read [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

1. <span data-ttu-id="4a143-131">Otevřete prostředí Azure PowerShell a přihlaste se tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="4a143-131">Open Azure PowerShell and sign in tooyour Azure account.</span></span> <span data-ttu-id="4a143-132">Automaticky otevírané okno otevře pro tooenter jste přihlašovací údaje účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="4a143-132">A pop-up window opens for you tooenter your Azure account credentials.</span></span>
   
    ```powershell
    Login-AzureRmAccount
    ```
2. <span data-ttu-id="4a143-133">Pořiďte si předplatné hello ID pro dostupných předplatných.</span><span class="sxs-lookup"><span data-stu-id="4a143-133">Get hello subscription IDs for your available subscriptions.</span></span>
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. <span data-ttu-id="4a143-134">Nastavit správné předplatné hello pomocí ID hello předplatného.</span><span class="sxs-lookup"><span data-stu-id="4a143-134">Set hello correct subscription using hello subscription ID.</span></span> <span data-ttu-id="4a143-135">Nahraďte  *<subscriptionID>*  s hello ID hello opravte předplatného.</span><span class="sxs-lookup"><span data-stu-id="4a143-135">Replace *<subscriptionID>* with hello ID of hello correct subscription.</span></span>
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="get-hello-storage-account"></a><span data-ttu-id="4a143-136">Získat účet úložiště hello</span><span class="sxs-lookup"><span data-stu-id="4a143-136">Get hello storage account</span></span>
<span data-ttu-id="4a143-137">Je nutné účet úložiště na image virtuálního počítače Azure toostore hello nahrát.</span><span class="sxs-lookup"><span data-stu-id="4a143-137">You need a storage account in Azure toostore hello uploaded VM image.</span></span> <span data-ttu-id="4a143-138">Můžete použít existující účet úložiště, nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="4a143-138">You can either use an existing storage account or create a new one.</span></span> 

<span data-ttu-id="4a143-139">Pokud budete používat hello virtuálního pevného disku toocreate spravovaných disků pro virtuální počítač, musí být umístění účtu úložiště hello stejné hello umístění, kde bude vytvářet hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4a143-139">If you will be using hello VHD toocreate a managed disk for a VM, hello storage account location must be same hello location where you will be creating hello VM.</span></span>

<span data-ttu-id="4a143-140">účty úložiště k dispozici hello tooshow, zadejte:</span><span class="sxs-lookup"><span data-stu-id="4a143-140">tooshow hello available storage accounts, type:</span></span>

```powershell
Get-AzureRmStorageAccount
```

<span data-ttu-id="4a143-141">Pokud chcete toouse stávající účet úložiště, pokračovat toohello [image virtuálního počítače hello nahrávání](#upload-the-vm-vhd-to-your-storage-account) části.</span><span class="sxs-lookup"><span data-stu-id="4a143-141">If you want toouse an existing storage account, proceed toohello [Upload hello VM image](#upload-the-vm-vhd-to-your-storage-account) section.</span></span>

<span data-ttu-id="4a143-142">Pokud potřebujete toocreate účet úložiště, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="4a143-142">If you need toocreate a storage account, follow these steps:</span></span>

1. <span data-ttu-id="4a143-143">Je nutné hello název skupiny prostředků hello, kde se vytvořit účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="4a143-143">You need hello name of hello resource group where hello storage account should be created.</span></span> <span data-ttu-id="4a143-144">toofind se všechny skupiny zdrojů hello, které jsou v rámci vašeho předplatného, typ:</span><span class="sxs-lookup"><span data-stu-id="4a143-144">toofind out all hello resource groups that are in your subscription, type:</span></span>
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    <span data-ttu-id="4a143-145">skupinu prostředků s názvem toocreate **myResourceGroup** v hello **východní USA** oblast, zadejte:</span><span class="sxs-lookup"><span data-stu-id="4a143-145">toocreate a resource group named **myResourceGroup** in hello **East US** region, type:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "East US"
    ```

2. <span data-ttu-id="4a143-146">Vytvořit účet úložiště s názvem **můj_účet_úložiště** v této skupině prostředků s použitím hello [AzureRmStorageAccount nový](/powershell/module/azurerm.storage/new-azurermstorageaccount) rutiny:</span><span class="sxs-lookup"><span data-stu-id="4a143-146">Create a storage account named **mystorageaccount** in this resource group by using hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span></span>
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "East US"`
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
   
    <span data-ttu-id="4a143-147">Platné hodnoty - SkuName jsou:</span><span class="sxs-lookup"><span data-stu-id="4a143-147">Valid values for -SkuName are:</span></span>
   
   * <span data-ttu-id="4a143-148">**Standard_LRS** -místně redundantní úložiště.</span><span class="sxs-lookup"><span data-stu-id="4a143-148">**Standard_LRS** - Locally redundant storage.</span></span> 
   * <span data-ttu-id="4a143-149">**Standard_ZRS** -zónu redundantní úložiště.</span><span class="sxs-lookup"><span data-stu-id="4a143-149">**Standard_ZRS** - Zone redundant storage.</span></span>
   * <span data-ttu-id="4a143-150">**Standard_GRS** -geograficky redundantní úložiště.</span><span class="sxs-lookup"><span data-stu-id="4a143-150">**Standard_GRS** - Geo redundant storage.</span></span> 
   * <span data-ttu-id="4a143-151">**Standard_RAGRS** -geograficky redundantní úložiště s přístupem pro čtení.</span><span class="sxs-lookup"><span data-stu-id="4a143-151">**Standard_RAGRS** - Read access geo redundant storage.</span></span> 
   * <span data-ttu-id="4a143-152">**Premium_LRS** -místně redundantního úložiště Premium.</span><span class="sxs-lookup"><span data-stu-id="4a143-152">**Premium_LRS** - Premium locally redundant storage.</span></span> 

## <a name="upload-hello-vhd-tooyour-storage-account"></a><span data-ttu-id="4a143-153">Nahrát účet úložiště tooyour hello virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="4a143-153">Upload hello VHD tooyour storage account</span></span>

<span data-ttu-id="4a143-154">Použití hello [přidat AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx) rutiny tooupload hello virtuálního pevného disku tooa kontejneru v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="4a143-154">Use hello [Add-AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx) cmdlet tooupload hello VHD tooa container in your storage account.</span></span> <span data-ttu-id="4a143-155">Tento příklad nahrávání hello souboru *myVHD.vhd* z *"C:\Users\Public\Documents\Virtual pevné disky\"*  tooa účet úložiště s názvem *můj_účet_úložiště*v hello *myResourceGroup* skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="4a143-155">This example uploads hello file *myVHD.vhd* from *"C:\Users\Public\Documents\Virtual hard disks\"* tooa storage account named *mystorageaccount* in hello *myResourceGroup* resource group.</span></span> <span data-ttu-id="4a143-156">soubor Hello budou umístěny do hello kontejner s názvem *můj_kontejner* a bude nový název souboru hello *myUploadedVHD.vhd*.</span><span class="sxs-lookup"><span data-stu-id="4a143-156">hello file will be placed into hello container named *mycontainer* and hello new file name will be *myUploadedVHD.vhd*.</span></span>

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


<span data-ttu-id="4a143-157">Pokud bylo úspěšné, můžete získat odpovědi, která vypadá podobně jako toothis:</span><span class="sxs-lookup"><span data-stu-id="4a143-157">If successful, you get a response that looks similar toothis:</span></span>

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

<span data-ttu-id="4a143-158">V závislosti na vašem síťovém připojení a hello velikost vašeho souboru virtuálního pevného disku, tento příkaz může chvíli trvat toocomplete</span><span class="sxs-lookup"><span data-stu-id="4a143-158">Depending on your network connection and hello size of your VHD file, this command may take a while toocomplete</span></span>

<span data-ttu-id="4a143-159">Uložit hello **identifikátor URI pro cíl** cesta toouse novější, pokud budete toocreate se spravovaným diskem nebo nového virtuálního počítače pomocí hello nahrát virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="4a143-159">Save hello **Destination URI** path toouse later if you are going toocreate a managed disk or a new VM using hello uploaded VHD.</span></span>

### <a name="other-options-for-uploading-a-vhd"></a><span data-ttu-id="4a143-160">Další možnosti pro odesílání virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="4a143-160">Other options for uploading a VHD</span></span>
 
 
<span data-ttu-id="4a143-161">Můžete také nahrát účtu úložiště tooyour virtuální pevný disk pomocí jedné z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="4a143-161">You can also upload a VHD tooyour storage account using one of hello following:</span></span>

- [<span data-ttu-id="4a143-162">AzCopy</span><span class="sxs-lookup"><span data-stu-id="4a143-162">AzCopy</span></span>](http://aka.ms/downloadazcopy)
- [<span data-ttu-id="4a143-163">Objekt Blob služby Azure Storage kopie rozhraní API</span><span class="sxs-lookup"><span data-stu-id="4a143-163">Azure Storage Copy Blob API</span></span>](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [<span data-ttu-id="4a143-164">Objektů BLOB Azure Storage Explorer odesílání</span><span class="sxs-lookup"><span data-stu-id="4a143-164">Azure Storage Explorer Uploading Blobs</span></span>](https://azurestorageexplorer.codeplex.com/)
- [<span data-ttu-id="4a143-165">Referenční dokumentace rozhraní API úložiště importu/exportu služby REST</span><span class="sxs-lookup"><span data-stu-id="4a143-165">Storage Import/Export Service REST API Reference</span></span>](https://msdn.microsoft.com/library/dn529096.aspx)
-   <span data-ttu-id="4a143-166">Doporučujeme používat službu Import/Export, pokud je předpokládaná doba odesílání doba je delší než 7 dní.</span><span class="sxs-lookup"><span data-stu-id="4a143-166">We recommend using Import/Export Service if estimated uploading time is longer than 7 days.</span></span> <span data-ttu-id="4a143-167">Můžete použít [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) čas hello tooestimate z jednotky velikost a přenos dat.</span><span class="sxs-lookup"><span data-stu-id="4a143-167">You can use [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) tooestimate hello time from data size and transfer unit.</span></span> 
    <span data-ttu-id="4a143-168">Import a Export lze použít toocopy tooa standardní účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="4a143-168">Import/Export can be used toocopy tooa standard storage account.</span></span> <span data-ttu-id="4a143-169">Budete potřebovat toocopy z účtu úložiště toopremium standardní úložiště pomocí nástroje, například AzCopy.</span><span class="sxs-lookup"><span data-stu-id="4a143-169">You will need toocopy from standard storage toopremium storage account using a tool like AzCopy.</span></span>


## <a name="create-a-managed-image-from-hello-uploaded-vhd"></a><span data-ttu-id="4a143-170">Vytvoření spravované bitovou kopii z hello nahrát virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="4a143-170">Create a managed image from hello uploaded VHD</span></span> 

<span data-ttu-id="4a143-171">Vytvoření spravované image pomocí vaší zobecněný virtuální pevný disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="4a143-171">Create a managed image using your generalized OS VHD.</span></span> <span data-ttu-id="4a143-172">Hello hodnoty nahraďte svými vlastními informacemi.</span><span class="sxs-lookup"><span data-stu-id="4a143-172">Replace hello values with your own information.</span></span>


1.  <span data-ttu-id="4a143-173">Nastavte nejprve, hello společné parametry:</span><span class="sxs-lookup"><span data-stu-id="4a143-173">First, set hello common parameters:</span></span>

    ```powershell
    $vmName = "myVM"
    $computerName = "myComputer"
    $vmSize = "Standard_DS1_v2"
    $location = "East US" 
    $imageName = "yourImageName"
    ```

4.  <span data-ttu-id="4a143-174">Vytvoření image hello pomocí vaší zobecněný virtuální pevný disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="4a143-174">Create hello image using your generalized OS VHD.</span></span>

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $urlOfUploadedImageVhd
    $image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```

## <a name="create-a-virtual-network"></a><span data-ttu-id="4a143-175">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="4a143-175">Create a virtual network</span></span>
<span data-ttu-id="4a143-176">Vytvoření hello virtuální síť a podsíť hello [virtuální sítě](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4a143-176">Create hello vNet and subnet of hello [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="4a143-177">Vytvořte podsíť hello.</span><span class="sxs-lookup"><span data-stu-id="4a143-177">Create hello subnet.</span></span> <span data-ttu-id="4a143-178">Tento příklad vytvoří podsíť s názvem *mySubnet* s předponou adresy hello z *10.0.0.0/24*.</span><span class="sxs-lookup"><span data-stu-id="4a143-178">This example creates a subnet named *mySubnet* with hello address prefix of *10.0.0.0/24*.</span></span>  
   
    ```powershell
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="4a143-179">Vytvoření virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="4a143-179">Create hello virtual network.</span></span> <span data-ttu-id="4a143-180">Tento příklad vytvoří virtuální síť s názvem *myVnet* s předponou adresy hello z *10.0.0.0/16*.</span><span class="sxs-lookup"><span data-stu-id="4a143-180">This example creates a virtual network named *myVnet* with hello address prefix of *10.0.0.0/16*.</span></span>  
   
    ```powershell
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-network-interface"></a><span data-ttu-id="4a143-181">Vytvoření veřejné IP adresy a síťového rozhraní</span><span class="sxs-lookup"><span data-stu-id="4a143-181">Create a public IP address and network interface</span></span>

<span data-ttu-id="4a143-182">tooenable komunikaci s hello virtuálním počítačem ve virtuální síti hello, musíte [veřejnou IP adresu](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) a síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="4a143-182">tooenable communication with hello virtual machine in hello virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="4a143-183">Vytvoření veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="4a143-183">Create a public IP address.</span></span> <span data-ttu-id="4a143-184">Tento příklad vytvoří veřejnou IP adresu s názvem *myPip*.</span><span class="sxs-lookup"><span data-stu-id="4a143-184">This example creates a public IP address named *myPip*.</span></span> 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="4a143-185">Vytvoření hello síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="4a143-185">Create hello NIC.</span></span> <span data-ttu-id="4a143-186">Tento příklad vytvoří síťový adaptér s názvem **myNic**.</span><span class="sxs-lookup"><span data-stu-id="4a143-186">This example creates a NIC named **myNic**.</span></span> 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-hello-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="4a143-187">Vytvořte skupinu zabezpečení sítě hello a pravidlo protokolu RDP</span><span class="sxs-lookup"><span data-stu-id="4a143-187">Create hello network security group and an RDP rule</span></span>

<span data-ttu-id="4a143-188">možnost toolog toobe v tooyour virtuálního počítače pomocí protokolu RDP, je nutné toohave pravidla zabezpečení sítě (NSG), který umožňuje přístup RDP na portu 3389.</span><span class="sxs-lookup"><span data-stu-id="4a143-188">toobe able toolog in tooyour VM using RDP, you need toohave a network security rule (NSG) that allows RDP access on port 3389.</span></span> 

<span data-ttu-id="4a143-189">Tento příklad vytvoří skupinu NSG s názvem *myNsg* obsahující pravidlo názvem *myRdpRule* přes port 3389 umožňuje provoz protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="4a143-189">This example creates an NSG named *myNsg* that contains a rule called *myRdpRule* that allows RDP traffic over port 3389.</span></span> <span data-ttu-id="4a143-190">Další informace o skupinách Nsg najdete v tématu [otevřít porty tooa virtuálních počítačů v Azure pomocí prostředí PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4a143-190">For more information about NSGs, see [Opening ports tooa VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

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


## <a name="create-a-variable-for-hello-virtual-network"></a><span data-ttu-id="4a143-191">Vytvořte proměnnou pro virtuální síť hello</span><span class="sxs-lookup"><span data-stu-id="4a143-191">Create a variable for hello virtual network</span></span>

<span data-ttu-id="4a143-192">Vytvořte proměnnou pro virtuální síť hello byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="4a143-192">Create a variable for hello completed virtual network.</span></span> 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

## <a name="get-hello-credentials-for-hello-vm"></a><span data-ttu-id="4a143-193">Získání přihlašovacích údajů hello pro hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="4a143-193">Get hello credentials for hello VM</span></span>

<span data-ttu-id="4a143-194">Hello následující rutiny se otevře okno, kde bude zadat nové toouse jméno a heslo uživatele jako účet místního správce hello pro vzdálený přístup k hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4a143-194">hello following cmdlet will open a window where you will enter a new user name and password toouse as hello local administrator account for remotely accessing hello VM.</span></span> 

```powershell
$cred = Get-Credential
```

## <a name="add-hello-vm-name-and-size-toohello-vm-configuration"></a><span data-ttu-id="4a143-195">Přidejte hello název virtuálního počítače a konfigurace virtuálního počítače toohello velikost.</span><span class="sxs-lookup"><span data-stu-id="4a143-195">Add hello VM name and size toohello VM configuration.</span></span>

```powershell
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

## <a name="set-hello-vm-image-as-source-image-for-hello-new-vm"></a><span data-ttu-id="4a143-196">Sada hello image virtuálního počítače jako zdroj bitové kopie pro hello nového virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="4a143-196">Set hello VM image as source image for hello new VM</span></span>

<span data-ttu-id="4a143-197">Nastavit hello zdrojové bitové kopie pomocí ID hello hello spravované image virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4a143-197">Set hello source image using hello ID of hello managed VM image.</span></span>

```powershell
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## <a name="set-hello-os-configuration-and-add-hello-nic"></a><span data-ttu-id="4a143-198">Nastavte hello konfigurace operačního systému a přidejte hello síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="4a143-198">Set hello OS configuration and add hello NIC.</span></span>

<span data-ttu-id="4a143-199">Zadejte typ úložiště hello (PremiumLRS nebo StandardLRS) a velikost hello hello disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="4a143-199">Enter hello storage type (PremiumLRS or StandardLRS) and hello size of hello OS disk.</span></span> <span data-ttu-id="4a143-200">Tento příklad nastaví typ účtu hello příliš*PremiumLRS*, hello velikost disku příliš*128 GB* a disku ukládání do mezipaměti příliš*ReadWrite*.</span><span class="sxs-lookup"><span data-stu-id="4a143-200">This example sets hello account type too*PremiumLRS*, hello disk size too*128 GB* and disk caching too*ReadWrite*.</span></span>

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## <a name="create-hello-vm"></a><span data-ttu-id="4a143-201">Vytvoření hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="4a143-201">Create hello VM</span></span>

<span data-ttu-id="4a143-202">Vytvoření nového virtuálního počítače pomocí hello konfigurace uložené v hello hello **$vm** proměnné.</span><span class="sxs-lookup"><span data-stu-id="4a143-202">Create hello new VM using hello configuration stored in hello **$vm** variable.</span></span>

```powershell
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

## <a name="verify-that-hello-vm-was-created"></a><span data-ttu-id="4a143-203">Ověřte, že hello, ke které byl virtuální počítač vytvořen</span><span class="sxs-lookup"><span data-stu-id="4a143-203">Verify that hello VM was created</span></span>
<span data-ttu-id="4a143-204">Po dokončení byste měli vidět hello nově vytvořený virtuální počítač v hello [portál Azure](https://portal.azure.com) pod **Procházet** > **virtuální počítače**, nebo pomocí následujících hello Příkazy prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="4a143-204">When complete, you should see hello newly created VM in hello [Azure portal](https://portal.azure.com) under **Browse** > **Virtual machines**, or by using hello following PowerShell commands:</span></span>

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="4a143-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4a143-205">Next steps</span></span>

<span data-ttu-id="4a143-206">toosign v tooyour nový virtuální počítač, procházet toohello virtuálních počítačů v hello [portál](https://portal.azure.com), klikněte na tlačítko **Connect**a soubor Remote Desktop RDP otevřete hello.</span><span class="sxs-lookup"><span data-stu-id="4a143-206">toosign in tooyour new virtual machine, browse toohello VM in hello [portal](https://portal.azure.com), click **Connect**, and open hello Remote Desktop RDP file.</span></span> <span data-ttu-id="4a143-207">Pomocí přihlašovacích údajů účtu hello z vašeho původního toosign virtuálního počítače v tooyour nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4a143-207">Use hello account credentials of your original virtual machine toosign in tooyour new virtual machine.</span></span> <span data-ttu-id="4a143-208">Další informace najdete v tématu [jak se tooconnect a protokol na tooan Azure virtuální počítač, systém Windows spuštěný](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4a143-208">For more information, see [How tooconnect and log on tooan Azure virtual machine running Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

