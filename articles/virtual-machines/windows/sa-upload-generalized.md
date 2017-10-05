---
title: "Nahrát generalizace virtuálního pevného disku pro vytvoření několika virtuálními počítači v Azure | Microsoft Docs"
description: "Nahrajte zobecněný virtuální pevný disk do účtu úložiště Azure k vytvoření virtuálního počítače s Windows pro použití s modelem nasazení Resource Manager."
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
ms.date: 05/18/2017
ms.author: cynthn
ms.openlocfilehash: e6fc49855b449a7723a7f8a0c1c41516b3a44ee5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="upload-a-generalized-vhd-to-azure-to-create-a-new-vm"></a><span data-ttu-id="b906a-103">Nahrajte zobecněný virtuální pevný disk do Azure k vytvoření nového virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b906a-103">Upload a generalized VHD to Azure to create a new VM</span></span>

<span data-ttu-id="b906a-104">Toto téma popisuje zobecněný nespravované disk se nahrávají na účet úložiště a pak vytvořit nový virtuální počítač pomocí nahrané disku.</span><span class="sxs-lookup"><span data-stu-id="b906a-104">This topic covers uploading a generalized unmanaged disk to a storage account and then creating a new VM using the uploaded disk.</span></span> <span data-ttu-id="b906a-105">Bitovou kopii zobecněný virtuální pevný disk má měly všechny vaše osobní účet informace odebrat pomocí nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="b906a-105">A generalized VHD image has had all of your personal account information removed using Sysprep.</span></span> 

<span data-ttu-id="b906a-106">Pokud chcete vytvořit virtuální počítač z specializované virtuálního pevného disku v účtu úložiště, najdete v části [vytvoření virtuálního počítače z disku VHD specializované](sa-create-vm-specialized.md).</span><span class="sxs-lookup"><span data-stu-id="b906a-106">If you want to create a VM from a specialized VHD in a storage account, see [Create a VM from a specialized VHD](sa-create-vm-specialized.md).</span></span>

<span data-ttu-id="b906a-107">Toto téma popisuje používání účtů úložiště, ale doporučujeme zákazníkům přesunutí místo toho použít spravované disky.</span><span class="sxs-lookup"><span data-stu-id="b906a-107">This topic covers using storage accounts, but we recommend customers move to using Managed Disks instead.</span></span> <span data-ttu-id="b906a-108">Kompletní návod jak připravit, odeslání a vytvoření nového virtuálního počítače pomocí spravovaných disků, najdete v části [vytvořit nový virtuální počítač z zobecněný virtuální pevný disk nahrán do Azure pomocí spravované disky](upload-generalized-managed.md).</span><span class="sxs-lookup"><span data-stu-id="b906a-108">For a complete walk-through of how to prepare, upload and create a new VM using managed disks, see [Create a new VM from a generalized VHD uploaded to Azure using Managed Disks](upload-generalized-managed.md).</span></span>



## <a name="prepare-the-vm"></a><span data-ttu-id="b906a-109">Příprava virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b906a-109">Prepare the VM</span></span>

<span data-ttu-id="b906a-110">Zobecněný virtuální pevný disk má měly všechny vaše osobní účet informace odebrat pomocí nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="b906a-110">A generalized VHD has had all of your personal account information removed using Sysprep.</span></span> <span data-ttu-id="b906a-111">Pokud máte v úmyslu použít virtuální pevný disk jako bitovou kopii k vytvoření nové virtuální počítače z, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b906a-111">If you intend to use the VHD as an image to create new VMs from, you should:</span></span>
  
  * <span data-ttu-id="b906a-112">[Příprava virtuálního pevného disku Windows nahrát do Azure](prepare-for-upload-vhd-image.md).</span><span class="sxs-lookup"><span data-stu-id="b906a-112">[Prepare a Windows VHD to upload to Azure](prepare-for-upload-vhd-image.md).</span></span> 
  * <span data-ttu-id="b906a-113">Virtuální počítač pomocí nástroje Sysprep generalize</span><span class="sxs-lookup"><span data-stu-id="b906a-113">Generalize the virtual machine using Sysprep</span></span>

### <a name="generalize-a-windows-virtual-machine-using-sysprep"></a><span data-ttu-id="b906a-114">Generalize virtuálního počítače s Windows pomocí nástroje Sysprep</span><span class="sxs-lookup"><span data-stu-id="b906a-114">Generalize a Windows virtual machine using Sysprep</span></span>
<span data-ttu-id="b906a-115">V této části se dozvíte, jak ke generalizaci virtuálního počítače Windows pro použití jako obrázek.</span><span class="sxs-lookup"><span data-stu-id="b906a-115">This section shows you how to generalize your Windows virtual machine for use as an image.</span></span> <span data-ttu-id="b906a-116">Nástroj Sysprep odstraní všechny vaše osobní informace o účtu, mimo jiné a připraví počítač, který se má použít jako obrázek.</span><span class="sxs-lookup"><span data-stu-id="b906a-116">Sysprep removes all your personal account information, among other things, and prepares the machine to be used as an image.</span></span> <span data-ttu-id="b906a-117">Podrobnosti o nástroji Sysprep najdete v tématu [postup použití nástroje Sysprep: Úvod](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="b906a-117">For details about Sysprep, see [How to Use Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>

<span data-ttu-id="b906a-118">Ujistěte se, že role serveru spuštěná na tomto počítači jsou podporovány nástrojem Sysprep.</span><span class="sxs-lookup"><span data-stu-id="b906a-118">Make sure the server roles running on the machine are supported by Sysprep.</span></span> <span data-ttu-id="b906a-119">Další informace najdete v tématu [podpora nástroje Sysprep pro role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span><span class="sxs-lookup"><span data-stu-id="b906a-119">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b906a-120">Pokud používáte nástroj Sysprep před nahráním svůj disk VHD do Azure poprvé, ujistěte se, máte [připravit virtuální počítač](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) před spuštěním nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="b906a-120">If you are running Sysprep before uploading your VHD to Azure for the first time, make sure you have [prepared your VM](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before running Sysprep.</span></span> 
> 
> 

1. <span data-ttu-id="b906a-121">Přihlaste se k virtuálnímu počítači Windows.</span><span class="sxs-lookup"><span data-stu-id="b906a-121">Sign in to the Windows virtual machine.</span></span>
2. <span data-ttu-id="b906a-122">Otevřete okno příkazového řádku jako správce.</span><span class="sxs-lookup"><span data-stu-id="b906a-122">Open the Command Prompt window as an administrator.</span></span> <span data-ttu-id="b906a-123">Změňte adresář na **%windir%\system32\sysprep**a poté spusťte `sysprep.exe`.</span><span class="sxs-lookup"><span data-stu-id="b906a-123">Change the directory to **%windir%\system32\sysprep**, and then run `sysprep.exe`.</span></span>
3. <span data-ttu-id="b906a-124">V **nástroj pro přípravu systému** dialogové okno, vyberte **prostředí Out-of-Box zadejte systému (při prvním zapnutí)**a ujistěte se, že **generalizace** je zaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="b906a-124">In the **System Preparation Tool** dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that the **Generalize** check box is selected.</span></span>
4. <span data-ttu-id="b906a-125">V **možnosti vypnutí**, vyberte **vypnutí**.</span><span class="sxs-lookup"><span data-stu-id="b906a-125">In **Shutdown Options**, select **Shutdown**.</span></span>
5. <span data-ttu-id="b906a-126">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="b906a-126">Click **OK**.</span></span>
   
    ![Spusťte nástroj Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. <span data-ttu-id="b906a-128">Po dokončení nástroj Sysprep vypne virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b906a-128">When Sysprep completes, it shuts down the virtual machine.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="b906a-129">Virtuální počítač nerestartuje, až do dokončení odesílání virtuálního pevného disku do Azure nebo vytvoření bitové kopie z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b906a-129">Do not restart the VM until you are done uploading the VHD to Azure or creating an image from the VM.</span></span> <span data-ttu-id="b906a-130">Pokud omylem získá restartování virtuálního počítače, spusťte nástroj Sysprep ke generalizaci ho znovu.</span><span class="sxs-lookup"><span data-stu-id="b906a-130">If the VM accidentally gets restarted, run Sysprep to generalize it again.</span></span>
> 
> 


## <a name="upload-the-vhd"></a><span data-ttu-id="b906a-131">Nahrání virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="b906a-131">Upload the VHD</span></span>

<span data-ttu-id="b906a-132">Nahrání virtuálního pevného disku do účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="b906a-132">Upload the VHD to an Azure storage account.</span></span>

### <a name="log-in-to-azure"></a><span data-ttu-id="b906a-133">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="b906a-133">Log in to Azure</span></span>
<span data-ttu-id="b906a-134">Pokud ještě nemáte prostředí PowerShell verze 1.4 nebo vyšší nainstalovaná, přečtěte si [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b906a-134">If you don't already have PowerShell version 1.4 or above installed, read [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

1. <span data-ttu-id="b906a-135">Otevřete prostředí Azure PowerShell a přihlaste se k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="b906a-135">Open Azure PowerShell and sign in to your Azure account.</span></span> <span data-ttu-id="b906a-136">Automaticky otevírané okno otevře zadat přihlašovací údaje účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="b906a-136">A pop-up window opens for you to enter your Azure account credentials.</span></span>
   
    ```powershell
    Login-AzureRmAccount
    ```
2. <span data-ttu-id="b906a-137">Pořiďte si předplatné ID pro dostupných předplatných.</span><span class="sxs-lookup"><span data-stu-id="b906a-137">Get the subscription IDs for your available subscriptions.</span></span>
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. <span data-ttu-id="b906a-138">Nastavit správné předplatné pomocí ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="b906a-138">Set the correct subscription using the subscription ID.</span></span> <span data-ttu-id="b906a-139">Nahraďte `<subscriptionID>` s ID správné předplatné.</span><span class="sxs-lookup"><span data-stu-id="b906a-139">Replace `<subscriptionID>` with the ID of the correct subscription.</span></span>
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

### <a name="get-the-storage-account"></a><span data-ttu-id="b906a-140">Získat účet úložiště</span><span class="sxs-lookup"><span data-stu-id="b906a-140">Get the storage account</span></span>
<span data-ttu-id="b906a-141">Potřebujete účet úložiště v Azure k ukládání nahrané image virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b906a-141">You need a storage account in Azure to store the uploaded VM image.</span></span> <span data-ttu-id="b906a-142">Můžete použít existující účet úložiště, nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="b906a-142">You can either use an existing storage account or create a new one.</span></span> 

<span data-ttu-id="b906a-143">Chcete-li zobrazit účty úložiště k dispozici, zadejte:</span><span class="sxs-lookup"><span data-stu-id="b906a-143">To show the available storage accounts, type:</span></span>

```powershell
Get-AzureRmStorageAccount
```

<span data-ttu-id="b906a-144">Pokud chcete použít existující účet úložiště, pokračujte [nahrajte image virtuálního počítače](#upload-the-vm-vhd-to-your-storage-account) části.</span><span class="sxs-lookup"><span data-stu-id="b906a-144">If you want to use an existing storage account, proceed to the [Upload the VM image](#upload-the-vm-vhd-to-your-storage-account) section.</span></span>

<span data-ttu-id="b906a-145">Pokud potřebujete vytvořit účet úložiště, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="b906a-145">If you need to create a storage account, follow these steps:</span></span>

1. <span data-ttu-id="b906a-146">Je třeba na název skupiny prostředků, kde by měl být vytvořen účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="b906a-146">You need the name of the resource group where the storage account should be created.</span></span> <span data-ttu-id="b906a-147">Chcete-li zjistit všechny skupiny prostředků, které jsou v rámci vašeho předplatného, zadejte:</span><span class="sxs-lookup"><span data-stu-id="b906a-147">To find out all the resource groups that are in your subscription, type:</span></span>
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    <span data-ttu-id="b906a-148">Chcete-li vytvořit skupinu prostředků s názvem **myResourceGroup** v **západní USA** oblast, zadejte:</span><span class="sxs-lookup"><span data-stu-id="b906a-148">To create a resource group named **myResourceGroup** in the **West US** region, type:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. <span data-ttu-id="b906a-149">Vytvořit účet úložiště s názvem **můj_účet_úložiště** v této skupině prostředků s použitím [AzureRmStorageAccount nový](/powershell/module/azurerm.storage/new-azurermstorageaccount) rutiny:</span><span class="sxs-lookup"><span data-stu-id="b906a-149">Create a storage account named **mystorageaccount** in this resource group by using the [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span></span>
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
 
### <a name="start-the-upload"></a><span data-ttu-id="b906a-150">Spusťte</span><span class="sxs-lookup"><span data-stu-id="b906a-150">Start the upload</span></span> 

<span data-ttu-id="b906a-151">Použití [přidat AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) rutiny Odeslat bitovou kopii do kontejneru v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="b906a-151">Use the [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet to upload the image to a container in your storage account.</span></span> <span data-ttu-id="b906a-152">Tento příklad nahrávání souboru **myVHD.vhd** z `"C:\Users\Public\Documents\Virtual hard disks\"` na účet úložiště s názvem **můj_účet_úložiště** v **myResourceGroup** skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="b906a-152">This example uploads the file **myVHD.vhd** from `"C:\Users\Public\Documents\Virtual hard disks\"` to a storage account named **mystorageaccount** in the **myResourceGroup** resource group.</span></span> <span data-ttu-id="b906a-153">Soubor se umístí do kontejner s názvem **můj_kontejner** a nový název souboru bude **myUploadedVHD.vhd**.</span><span class="sxs-lookup"><span data-stu-id="b906a-153">The file will be placed into the container named **mycontainer** and the new file name will be **myUploadedVHD.vhd**.</span></span>

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


<span data-ttu-id="b906a-154">Pokud bylo úspěšné, můžete získat odpovědi, která vypadá podobně jako tento:</span><span class="sxs-lookup"><span data-stu-id="b906a-154">If successful, you get a response that looks similar to this:</span></span>

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

<span data-ttu-id="b906a-155">V závislosti na připojení k síti a velikost souboru virtuálního pevného disku tento příkaz může trvat nějakou dobu pro dokončení.</span><span class="sxs-lookup"><span data-stu-id="b906a-155">Depending on your network connection and the size of your VHD file, this command may take a while to complete.</span></span>


## <a name="create-a-new-vm"></a><span data-ttu-id="b906a-156">Vytvoření nového virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b906a-156">Create a new VM</span></span> 

<span data-ttu-id="b906a-157">Nyní můžete nahraný virtuální pevný disk pro vytvoření nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b906a-157">You can now use the uploaded VHD to create a new VM.</span></span> 

### <a name="set-the-uri-of-the-vhd"></a><span data-ttu-id="b906a-158">Nastavit identifikátor URI virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="b906a-158">Set the URI of the VHD</span></span>

<span data-ttu-id="b906a-159">Identifikátor URI pro VHD, který chcete používat je ve formátu: https://**můj_účet_úložiště**.blob.core.windows.net/**můj_kontejner**/**MyVhdName**VHD.</span><span class="sxs-lookup"><span data-stu-id="b906a-159">The URI for the VHD to use is in the format: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**.vhd.</span></span> <span data-ttu-id="b906a-160">V tomto příkladu virtuální pevný disk s názvem **myVHD** je v účtu úložiště **můj_účet_úložiště** v kontejneru **můj_kontejner**.</span><span class="sxs-lookup"><span data-stu-id="b906a-160">In this example the VHD named **myVHD** is in the storage account **mystorageaccount** in the container **mycontainer**.</span></span>

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


### <a name="create-a-virtual-network"></a><span data-ttu-id="b906a-161">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="b906a-161">Create a virtual network</span></span>
<span data-ttu-id="b906a-162">Vytvořit virtuální síť a podsíť [virtuální sítě](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b906a-162">Create the vNet and subnet of the [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="b906a-163">Vytvořte podsíť.</span><span class="sxs-lookup"><span data-stu-id="b906a-163">Create the subnet.</span></span> <span data-ttu-id="b906a-164">Následující příklad vytvoří podsíť s názvem **mySubnet** ve skupině prostředků **myResourceGroup** s předponou adresy z **10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="b906a-164">The following sample creates a subnet named **mySubnet** in the resource group **myResourceGroup** with the address prefix of **10.0.0.0/24**.</span></span>  
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="b906a-165">Vytvořte virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="b906a-165">Create the virtual network.</span></span> <span data-ttu-id="b906a-166">Následující ukázka vytvoří virtuální síť s názvem **myVnet** v **západní USA** umístění s předponou adresy z **10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="b906a-166">The following sample creates a virtual network named **myVnet** in the **West US** location with the address prefix of **10.0.0.0/16**.</span></span>  
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-network-interface"></a><span data-ttu-id="b906a-167">Vytvoření veřejné IP adresy a síťového rozhraní</span><span class="sxs-lookup"><span data-stu-id="b906a-167">Create a public IP address and network interface</span></span>
<span data-ttu-id="b906a-168">Pokud chcete povolit komunikaci s virtuálním počítačem ve virtuální síti, budete potřebovat [veřejnou adresu IP](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) a síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="b906a-168">To enable communication with the virtual machine in the virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="b906a-169">Vytvoření veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="b906a-169">Create a public IP address.</span></span> <span data-ttu-id="b906a-170">Tento příklad vytvoří veřejnou IP adresu s názvem **myPip**.</span><span class="sxs-lookup"><span data-stu-id="b906a-170">This example creates a public IP address named **myPip**.</span></span> 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="b906a-171">Vytvořit síťovou kartu.</span><span class="sxs-lookup"><span data-stu-id="b906a-171">Create the NIC.</span></span> <span data-ttu-id="b906a-172">Tento příklad vytvoří síťový adaptér s názvem **myNic**.</span><span class="sxs-lookup"><span data-stu-id="b906a-172">This example creates a NIC named **myNic**.</span></span> 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-the-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="b906a-173">Vytvořte skupinu zabezpečení sítě a pravidlo protokolu RDP</span><span class="sxs-lookup"><span data-stu-id="b906a-173">Create the network security group and an RDP rule</span></span>
<span data-ttu-id="b906a-174">Abyste mohli přihlásit k virtuálnímu počítači pomocí protokolu RDP, musíte mít pravidlo zabezpečení, která umožňuje přístup RDP na portu 3389.</span><span class="sxs-lookup"><span data-stu-id="b906a-174">To be able to log in to your VM using RDP, you need to have a security rule that allows RDP access on port 3389.</span></span> 

<span data-ttu-id="b906a-175">Tento příklad vytvoří skupinu NSG s názvem **myNsg** obsahující pravidlo názvem **myRdpRule** přes port 3389 umožňuje provoz protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="b906a-175">This example creates an NSG named **myNsg** that contains a rule called **myRdpRule** that allows RDP traffic over port 3389.</span></span> <span data-ttu-id="b906a-176">Další informace o skupinách Nsg najdete v tématu [otevřít porty pro virtuální počítač v Azure pomocí prostředí PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b906a-176">For more information about NSGs, see [Opening ports to a VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


### <a name="create-a-variable-for-the-virtual-network"></a><span data-ttu-id="b906a-177">Vytvořte proměnnou pro virtuální síť</span><span class="sxs-lookup"><span data-stu-id="b906a-177">Create a variable for the virtual network</span></span>
<span data-ttu-id="b906a-178">Vytvořte proměnnou pro dokončené virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="b906a-178">Create a variable for the completed virtual network.</span></span> 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

### <a name="create-the-vm"></a><span data-ttu-id="b906a-179">Vytvořte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b906a-179">Create the VM</span></span>
<span data-ttu-id="b906a-180">Následující skript prostředí PowerShell ukazuje, jak nastavit konfiguraci virtuálního počítače a použít nahrané image virtuálního počítače jako zdroj pro novou instalaci.</span><span class="sxs-lookup"><span data-stu-id="b906a-180">The following PowerShell script shows how to set up the virtual machine configurations and use the uploaded VM image as the source for the new installation.</span></span>



```powershell
# Enter a new user name and password to use as the local administrator account 
    # for remotely accessing the VM.
    $cred = Get-Credential

    # Name of the storage account where the VHD is located. This example sets the 
    # storage account name as "myStorageAccount"
    $storageAccName = "myStorageAccount"

    # Name of the virtual machine. This example sets the VM name as "myVM".
    $vmName = "myVM"

    # Size of the virtual machine. This example creates "Standard_D2_v2" sized VM. 
    # See the VM sizes documentation for more information: 
    # https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "Standard_D2_v2"

    # Computer name for the VM. This examples sets the computer name as "myComputer".
    $computerName = "myComputer"

    # Name of the disk that holds the OS. This example sets the 
    # OS disk name as "myOsDisk"
    $osDiskName = "myOsDisk"

    # Assign a SKU name. This example sets the SKU name as "Standard_LRS"
    # Valid values for -SkuName are: Standard_LRS - locally redundant storage, Standard_ZRS - zone redundant
    # storage, Standard_GRS - geo redundant storage, Standard_RAGRS - read access geo redundant storage,
    # Premium_LRS - premium locally redundant storage. 
    $skuName = "Standard_LRS"

    # Get the storage account where the uploaded image is stored
    $storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName

    # Set the VM name and size
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize

    #Set the Windows operating system configuration and add the NIC
    $vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id

    # Create the OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName

    # Configure the OS disk to be created from the existing VHD image (-CreateOption fromImage).
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows

    # Create the new VM
    New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

## <a name="verify-that-the-vm-was-created"></a><span data-ttu-id="b906a-181">Zkontrolujte, zda byl vytvořen virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="b906a-181">Verify that the VM was created</span></span>
<span data-ttu-id="b906a-182">Po dokončení byste měli vidět nově vytvořený virtuální počítač v [portál Azure](https://portal.azure.com) pod **Procházet** > **virtuální počítače**, nebo pomocí následujících příkazů prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="b906a-182">When complete, you should see the newly created VM in the [Azure portal](https://portal.azure.com) under **Browse** > **Virtual machines**, or by using the following PowerShell commands:</span></span>

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="b906a-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b906a-183">Next steps</span></span>
<span data-ttu-id="b906a-184">Ke správě nového virtuálního počítače v prostředí Azure PowerShell najdete [Správa virtuálních počítačů pomocí Azure Resource Manageru a prostředí PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b906a-184">To manage your new virtual machine with Azure PowerShell, see [Manage virtual machines using Azure Resource Manager and PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


