---
title: "aaaUpload toocreate virtuálního pevného disku generalizace více virtuálních počítačů v Azure | Microsoft Docs"
description: "Nahrajte zobecněný virtuální pevný disk tooan úložiště Azure účet toocreate toouse virtuální počítač s Windows pomocí modelu nasazení Resource Manager hello."
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
ms.openlocfilehash: aa1af2a0acf81685e62853de71afa51e819cb696
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-a-generalized-vhd-tooazure-toocreate-a-new-vm"></a><span data-ttu-id="4981c-103">Nahrát zobecněný toocreate tooAzure virtuální pevný disk nového virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="4981c-103">Upload a generalized VHD tooAzure toocreate a new VM</span></span>

<span data-ttu-id="4981c-104">Toto téma popisuje odesílání účtu úložiště tooa zobecněný nespravované disk a pak vytvořit nový virtuální počítač pomocí disku hello nahrát.</span><span class="sxs-lookup"><span data-stu-id="4981c-104">This topic covers uploading a generalized unmanaged disk tooa storage account and then creating a new VM using hello uploaded disk.</span></span> <span data-ttu-id="4981c-105">Bitovou kopii zobecněný virtuální pevný disk má měly všechny vaše osobní účet informace odebrat pomocí nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="4981c-105">A generalized VHD image has had all of your personal account information removed using Sysprep.</span></span> 

<span data-ttu-id="4981c-106">Pokud chcete toocreate virtuálního počítače z specializované virtuálního pevného disku v účtu úložiště, najdete v části [vytvoření virtuálního počítače z disku VHD specializované](sa-create-vm-specialized.md).</span><span class="sxs-lookup"><span data-stu-id="4981c-106">If you want toocreate a VM from a specialized VHD in a storage account, see [Create a VM from a specialized VHD](sa-create-vm-specialized.md).</span></span>

<span data-ttu-id="4981c-107">Toto téma popisuje používání účtů úložiště, ale doporučujeme, aby zákazníci přesunout disky toousing spravované místo.</span><span class="sxs-lookup"><span data-stu-id="4981c-107">This topic covers using storage accounts, but we recommend customers move toousing Managed Disks instead.</span></span> <span data-ttu-id="4981c-108">Kompletní návod jak tooprepare, odesílání a vytvořit nový virtuální počítač pomocí správy disků najdete v části [vytvořit nový virtuální počítač z zobecněný virtuální pevný disk nahrán tooAzure pomocí disků spravované](upload-generalized-managed.md).</span><span class="sxs-lookup"><span data-stu-id="4981c-108">For a complete walk-through of how tooprepare, upload and create a new VM using managed disks, see [Create a new VM from a generalized VHD uploaded tooAzure using Managed Disks](upload-generalized-managed.md).</span></span>



## <a name="prepare-hello-vm"></a><span data-ttu-id="4981c-109">Příprava hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="4981c-109">Prepare hello VM</span></span>

<span data-ttu-id="4981c-110">Zobecněný virtuální pevný disk má měly všechny vaše osobní účet informace odebrat pomocí nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="4981c-110">A generalized VHD has had all of your personal account information removed using Sysprep.</span></span> <span data-ttu-id="4981c-111">Pokud máte v úmyslu toouse hello virtuálního pevného disku jako image toocreate nové virtuální počítače z, měli byste:</span><span class="sxs-lookup"><span data-stu-id="4981c-111">If you intend toouse hello VHD as an image toocreate new VMs from, you should:</span></span>
  
  * <span data-ttu-id="4981c-112">[Příprava virtuální pevný disk Windows tooupload tooAzure](prepare-for-upload-vhd-image.md).</span><span class="sxs-lookup"><span data-stu-id="4981c-112">[Prepare a Windows VHD tooupload tooAzure](prepare-for-upload-vhd-image.md).</span></span> 
  * <span data-ttu-id="4981c-113">Generalize hello virtuálního počítače pomocí nástroje Sysprep</span><span class="sxs-lookup"><span data-stu-id="4981c-113">Generalize hello virtual machine using Sysprep</span></span>

### <a name="generalize-a-windows-virtual-machine-using-sysprep"></a><span data-ttu-id="4981c-114">Generalize virtuálního počítače s Windows pomocí nástroje Sysprep</span><span class="sxs-lookup"><span data-stu-id="4981c-114">Generalize a Windows virtual machine using Sysprep</span></span>
<span data-ttu-id="4981c-115">V této části se dozvíte, jak toogeneralize virtuálního počítače Windows pro použití jako obrázek.</span><span class="sxs-lookup"><span data-stu-id="4981c-115">This section shows you how toogeneralize your Windows virtual machine for use as an image.</span></span> <span data-ttu-id="4981c-116">Nástroj Sysprep odstraní všechny vaše osobní informace o účtu, mimo jiné a připraví toobe počítač hello použít jako obrázek.</span><span class="sxs-lookup"><span data-stu-id="4981c-116">Sysprep removes all your personal account information, among other things, and prepares hello machine toobe used as an image.</span></span> <span data-ttu-id="4981c-117">Podrobnosti o nástroji Sysprep najdete v tématu [jak tooUse nástroje Sysprep: Úvod](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="4981c-117">For details about Sysprep, see [How tooUse Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>

<span data-ttu-id="4981c-118">Ujistěte se, že role serveru hello spuštěné na počítači hello jsou podporovány nástrojem Sysprep.</span><span class="sxs-lookup"><span data-stu-id="4981c-118">Make sure hello server roles running on hello machine are supported by Sysprep.</span></span> <span data-ttu-id="4981c-119">Další informace najdete v tématu [podpora nástroje Sysprep pro role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span><span class="sxs-lookup"><span data-stu-id="4981c-119">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4981c-120">Pokud používáte nástroj Sysprep před nahráním vaší tooAzure virtuálního pevného disku pro hello poprvé, ujistěte se, máte [připravit virtuální počítač](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) před spuštěním nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="4981c-120">If you are running Sysprep before uploading your VHD tooAzure for hello first time, make sure you have [prepared your VM](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before running Sysprep.</span></span> 
> 
> 

1. <span data-ttu-id="4981c-121">Přihlaste se toohello virtuálního počítače Windows.</span><span class="sxs-lookup"><span data-stu-id="4981c-121">Sign in toohello Windows virtual machine.</span></span>
2. <span data-ttu-id="4981c-122">Otevřete okno příkazového řádku hello jako správce.</span><span class="sxs-lookup"><span data-stu-id="4981c-122">Open hello Command Prompt window as an administrator.</span></span> <span data-ttu-id="4981c-123">Změnit adresář, hello příliš**%windir%\system32\sysprep**a poté spusťte `sysprep.exe`.</span><span class="sxs-lookup"><span data-stu-id="4981c-123">Change hello directory too**%windir%\system32\sysprep**, and then run `sysprep.exe`.</span></span>
3. <span data-ttu-id="4981c-124">V hello **nástroj pro přípravu systému** dialogové okno, vyberte **prostředí Out-of-Box zadejte systému (při prvním zapnutí)**a ujistěte se, že hello **generalizace** je zaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="4981c-124">In hello **System Preparation Tool** dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that hello **Generalize** check box is selected.</span></span>
4. <span data-ttu-id="4981c-125">V **možnosti vypnutí**, vyberte **vypnutí**.</span><span class="sxs-lookup"><span data-stu-id="4981c-125">In **Shutdown Options**, select **Shutdown**.</span></span>
5. <span data-ttu-id="4981c-126">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="4981c-126">Click **OK**.</span></span>
   
    ![Spusťte nástroj Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. <span data-ttu-id="4981c-128">Po dokončení nástroj Sysprep vypne hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4981c-128">When Sysprep completes, it shuts down hello virtual machine.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="4981c-129">Nerestartovat hello virtuálních počítačů, dokud jste done odesílání tooAzure hello virtuálního pevného disku nebo vytvoření bitové kopie z hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4981c-129">Do not restart hello VM until you are done uploading hello VHD tooAzure or creating an image from hello VM.</span></span> <span data-ttu-id="4981c-130">Pokud omylem získá restartována hello virtuálního počítače, spusťte nástroj Sysprep toogeneralize ho znovu.</span><span class="sxs-lookup"><span data-stu-id="4981c-130">If hello VM accidentally gets restarted, run Sysprep toogeneralize it again.</span></span>
> 
> 


## <a name="upload-hello-vhd"></a><span data-ttu-id="4981c-131">Nahrát hello virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="4981c-131">Upload hello VHD</span></span>

<span data-ttu-id="4981c-132">Nahrajte účtu úložiště Azure tooan hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="4981c-132">Upload hello VHD tooan Azure storage account.</span></span>

### <a name="log-in-tooazure"></a><span data-ttu-id="4981c-133">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="4981c-133">Log in tooAzure</span></span>
<span data-ttu-id="4981c-134">Pokud ještě nemáte prostředí PowerShell verze 1.4 nebo vyšší nainstalovaná, přečtěte si [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4981c-134">If you don't already have PowerShell version 1.4 or above installed, read [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

1. <span data-ttu-id="4981c-135">Otevřete prostředí Azure PowerShell a přihlaste se tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="4981c-135">Open Azure PowerShell and sign in tooyour Azure account.</span></span> <span data-ttu-id="4981c-136">Automaticky otevírané okno otevře pro tooenter jste přihlašovací údaje účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="4981c-136">A pop-up window opens for you tooenter your Azure account credentials.</span></span>
   
    ```powershell
    Login-AzureRmAccount
    ```
2. <span data-ttu-id="4981c-137">Pořiďte si předplatné hello ID pro dostupných předplatných.</span><span class="sxs-lookup"><span data-stu-id="4981c-137">Get hello subscription IDs for your available subscriptions.</span></span>
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. <span data-ttu-id="4981c-138">Nastavit správné předplatné hello pomocí ID hello předplatného.</span><span class="sxs-lookup"><span data-stu-id="4981c-138">Set hello correct subscription using hello subscription ID.</span></span> <span data-ttu-id="4981c-139">Nahraďte `<subscriptionID>` s hello ID hello opravte předplatného.</span><span class="sxs-lookup"><span data-stu-id="4981c-139">Replace `<subscriptionID>` with hello ID of hello correct subscription.</span></span>
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

### <a name="get-hello-storage-account"></a><span data-ttu-id="4981c-140">Získat účet úložiště hello</span><span class="sxs-lookup"><span data-stu-id="4981c-140">Get hello storage account</span></span>
<span data-ttu-id="4981c-141">Je nutné účet úložiště na image virtuálního počítače Azure toostore hello nahrát.</span><span class="sxs-lookup"><span data-stu-id="4981c-141">You need a storage account in Azure toostore hello uploaded VM image.</span></span> <span data-ttu-id="4981c-142">Můžete použít existující účet úložiště, nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="4981c-142">You can either use an existing storage account or create a new one.</span></span> 

<span data-ttu-id="4981c-143">účty úložiště k dispozici hello tooshow, zadejte:</span><span class="sxs-lookup"><span data-stu-id="4981c-143">tooshow hello available storage accounts, type:</span></span>

```powershell
Get-AzureRmStorageAccount
```

<span data-ttu-id="4981c-144">Pokud chcete toouse stávající účet úložiště, pokračovat toohello [image virtuálního počítače hello nahrávání](#upload-the-vm-vhd-to-your-storage-account) části.</span><span class="sxs-lookup"><span data-stu-id="4981c-144">If you want toouse an existing storage account, proceed toohello [Upload hello VM image](#upload-the-vm-vhd-to-your-storage-account) section.</span></span>

<span data-ttu-id="4981c-145">Pokud potřebujete toocreate účet úložiště, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="4981c-145">If you need toocreate a storage account, follow these steps:</span></span>

1. <span data-ttu-id="4981c-146">Je nutné hello název skupiny prostředků hello, kde se vytvořit účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="4981c-146">You need hello name of hello resource group where hello storage account should be created.</span></span> <span data-ttu-id="4981c-147">toofind se všechny skupiny zdrojů hello, které jsou v rámci vašeho předplatného, typ:</span><span class="sxs-lookup"><span data-stu-id="4981c-147">toofind out all hello resource groups that are in your subscription, type:</span></span>
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    <span data-ttu-id="4981c-148">skupinu prostředků s názvem toocreate **myResourceGroup** v hello **západní USA** oblast, zadejte:</span><span class="sxs-lookup"><span data-stu-id="4981c-148">toocreate a resource group named **myResourceGroup** in hello **West US** region, type:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. <span data-ttu-id="4981c-149">Vytvořit účet úložiště s názvem **můj_účet_úložiště** v této skupině prostředků s použitím hello [AzureRmStorageAccount nový](/powershell/module/azurerm.storage/new-azurermstorageaccount) rutiny:</span><span class="sxs-lookup"><span data-stu-id="4981c-149">Create a storage account named **mystorageaccount** in this resource group by using hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span></span>
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
 
### <a name="start-hello-upload"></a><span data-ttu-id="4981c-150">Spustit odeslání hello</span><span class="sxs-lookup"><span data-stu-id="4981c-150">Start hello upload</span></span> 

<span data-ttu-id="4981c-151">Použití hello [přidat AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) rutiny tooupload hello image tooa kontejneru v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="4981c-151">Use hello [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet tooupload hello image tooa container in your storage account.</span></span> <span data-ttu-id="4981c-152">Tento příklad nahrávání hello souboru **myVHD.vhd** z `"C:\Users\Public\Documents\Virtual hard disks\"` tooa účet úložiště s názvem **můj_účet_úložiště** v hello **myResourceGroup** skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="4981c-152">This example uploads hello file **myVHD.vhd** from `"C:\Users\Public\Documents\Virtual hard disks\"` tooa storage account named **mystorageaccount** in hello **myResourceGroup** resource group.</span></span> <span data-ttu-id="4981c-153">soubor Hello budou umístěny do hello kontejner s názvem **můj_kontejner** a bude nový název souboru hello **myUploadedVHD.vhd**.</span><span class="sxs-lookup"><span data-stu-id="4981c-153">hello file will be placed into hello container named **mycontainer** and hello new file name will be **myUploadedVHD.vhd**.</span></span>

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


<span data-ttu-id="4981c-154">Pokud bylo úspěšné, můžete získat odpovědi, která vypadá podobně jako toothis:</span><span class="sxs-lookup"><span data-stu-id="4981c-154">If successful, you get a response that looks similar toothis:</span></span>

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

<span data-ttu-id="4981c-155">V závislosti na vašem síťovém připojení a hello velikost vašeho souboru virtuálního pevného disku, tento příkaz může chvíli trvat toocomplete.</span><span class="sxs-lookup"><span data-stu-id="4981c-155">Depending on your network connection and hello size of your VHD file, this command may take a while toocomplete.</span></span>


## <a name="create-a-new-vm"></a><span data-ttu-id="4981c-156">Vytvoření nového virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="4981c-156">Create a new VM</span></span> 

<span data-ttu-id="4981c-157">Můžete se teď může použít hello nahrán toocreate virtuální pevný disk nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4981c-157">You can now use hello uploaded VHD toocreate a new VM.</span></span> 

### <a name="set-hello-uri-of-hello-vhd"></a><span data-ttu-id="4981c-158">Nastavit hello URI hello virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="4981c-158">Set hello URI of hello VHD</span></span>

<span data-ttu-id="4981c-159">Hello identifikátor URI pro toouse hello virtuální pevný disk je ve formátu hello: https://**můj_účet_úložiště**.blob.core.windows.net/**můj_kontejner**/**MyVhdName**VHD.</span><span class="sxs-lookup"><span data-stu-id="4981c-159">hello URI for hello VHD toouse is in hello format: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**.vhd.</span></span> <span data-ttu-id="4981c-160">V tomto příkladu hello virtuálního pevného disku s názvem **myVHD** v účtu úložiště hello **můj_účet_úložiště** v kontejneru hello **můj_kontejner**.</span><span class="sxs-lookup"><span data-stu-id="4981c-160">In this example hello VHD named **myVHD** is in hello storage account **mystorageaccount** in hello container **mycontainer**.</span></span>

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


### <a name="create-a-virtual-network"></a><span data-ttu-id="4981c-161">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="4981c-161">Create a virtual network</span></span>
<span data-ttu-id="4981c-162">Vytvoření hello virtuální síť a podsíť hello [virtuální sítě](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4981c-162">Create hello vNet and subnet of hello [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="4981c-163">Vytvořte podsíť hello.</span><span class="sxs-lookup"><span data-stu-id="4981c-163">Create hello subnet.</span></span> <span data-ttu-id="4981c-164">Hello následující ukázka vytvoří podsíť s názvem **mySubnet** ve skupině prostředků hello **myResourceGroup** s předponou adresy hello z **10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="4981c-164">hello following sample creates a subnet named **mySubnet** in hello resource group **myResourceGroup** with hello address prefix of **10.0.0.0/24**.</span></span>  
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="4981c-165">Vytvoření virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="4981c-165">Create hello virtual network.</span></span> <span data-ttu-id="4981c-166">Hello následující ukázka vytvoří virtuální síť s názvem **myVnet** v hello **západní USA** umístění s předponu adresy hello **10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="4981c-166">hello following sample creates a virtual network named **myVnet** in hello **West US** location with hello address prefix of **10.0.0.0/16**.</span></span>  
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-network-interface"></a><span data-ttu-id="4981c-167">Vytvoření veřejné IP adresy a síťového rozhraní</span><span class="sxs-lookup"><span data-stu-id="4981c-167">Create a public IP address and network interface</span></span>
<span data-ttu-id="4981c-168">tooenable komunikaci s hello virtuálním počítačem ve virtuální síti hello, musíte [veřejnou IP adresu](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) a síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="4981c-168">tooenable communication with hello virtual machine in hello virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="4981c-169">Vytvoření veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="4981c-169">Create a public IP address.</span></span> <span data-ttu-id="4981c-170">Tento příklad vytvoří veřejnou IP adresu s názvem **myPip**.</span><span class="sxs-lookup"><span data-stu-id="4981c-170">This example creates a public IP address named **myPip**.</span></span> 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="4981c-171">Vytvoření hello síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="4981c-171">Create hello NIC.</span></span> <span data-ttu-id="4981c-172">Tento příklad vytvoří síťový adaptér s názvem **myNic**.</span><span class="sxs-lookup"><span data-stu-id="4981c-172">This example creates a NIC named **myNic**.</span></span> 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-hello-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="4981c-173">Vytvořte skupinu zabezpečení sítě hello a pravidlo protokolu RDP</span><span class="sxs-lookup"><span data-stu-id="4981c-173">Create hello network security group and an RDP rule</span></span>
<span data-ttu-id="4981c-174">možnost toolog toobe v tooyour virtuálního počítače pomocí protokolu RDP, je nutné toohave pravidlo zabezpečení, která umožňuje přístup RDP na portu 3389.</span><span class="sxs-lookup"><span data-stu-id="4981c-174">toobe able toolog in tooyour VM using RDP, you need toohave a security rule that allows RDP access on port 3389.</span></span> 

<span data-ttu-id="4981c-175">Tento příklad vytvoří skupinu NSG s názvem **myNsg** obsahující pravidlo názvem **myRdpRule** přes port 3389 umožňuje provoz protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="4981c-175">This example creates an NSG named **myNsg** that contains a rule called **myRdpRule** that allows RDP traffic over port 3389.</span></span> <span data-ttu-id="4981c-176">Další informace o skupinách Nsg najdete v tématu [otevřít porty tooa virtuálních počítačů v Azure pomocí prostředí PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4981c-176">For more information about NSGs, see [Opening ports tooa VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


### <a name="create-a-variable-for-hello-virtual-network"></a><span data-ttu-id="4981c-177">Vytvořte proměnnou pro virtuální síť hello</span><span class="sxs-lookup"><span data-stu-id="4981c-177">Create a variable for hello virtual network</span></span>
<span data-ttu-id="4981c-178">Vytvořte proměnnou pro virtuální síť hello byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="4981c-178">Create a variable for hello completed virtual network.</span></span> 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

### <a name="create-hello-vm"></a><span data-ttu-id="4981c-179">Vytvoření hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="4981c-179">Create hello VM</span></span>
<span data-ttu-id="4981c-180">Hello následující skript prostředí PowerShell ukazuje, jak tooset hello konfigurace virtuálních počítačů a použití hello odesílané image virtuálního počítače jako hello zdroj pro novou instalaci hello.</span><span class="sxs-lookup"><span data-stu-id="4981c-180">hello following PowerShell script shows how tooset up hello virtual machine configurations and use hello uploaded VM image as hello source for hello new installation.</span></span>



```powershell
# Enter a new user name and password toouse as hello local administrator account 
    # for remotely accessing hello VM.
    $cred = Get-Credential

    # Name of hello storage account where hello VHD is located. This example sets hello 
    # storage account name as "myStorageAccount"
    $storageAccName = "myStorageAccount"

    # Name of hello virtual machine. This example sets hello VM name as "myVM".
    $vmName = "myVM"

    # Size of hello virtual machine. This example creates "Standard_D2_v2" sized VM. 
    # See hello VM sizes documentation for more information: 
    # https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "Standard_D2_v2"

    # Computer name for hello VM. This examples sets hello computer name as "myComputer".
    $computerName = "myComputer"

    # Name of hello disk that holds hello OS. This example sets hello 
    # OS disk name as "myOsDisk"
    $osDiskName = "myOsDisk"

    # Assign a SKU name. This example sets hello SKU name as "Standard_LRS"
    # Valid values for -SkuName are: Standard_LRS - locally redundant storage, Standard_ZRS - zone redundant
    # storage, Standard_GRS - geo redundant storage, Standard_RAGRS - read access geo redundant storage,
    # Premium_LRS - premium locally redundant storage. 
    $skuName = "Standard_LRS"

    # Get hello storage account where hello uploaded image is stored
    $storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName

    # Set hello VM name and size
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize

    #Set hello Windows operating system configuration and add hello NIC
    $vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id

    # Create hello OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName

    # Configure hello OS disk toobe created from hello existing VHD image (-CreateOption fromImage).
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows

    # Create hello new VM
    New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

## <a name="verify-that-hello-vm-was-created"></a><span data-ttu-id="4981c-181">Ověřte, že hello, ke které byl virtuální počítač vytvořen</span><span class="sxs-lookup"><span data-stu-id="4981c-181">Verify that hello VM was created</span></span>
<span data-ttu-id="4981c-182">Po dokončení byste měli vidět hello nově vytvořený virtuální počítač v hello [portál Azure](https://portal.azure.com) pod **Procházet** > **virtuální počítače**, nebo pomocí následujících hello Příkazy prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="4981c-182">When complete, you should see hello newly created VM in hello [Azure portal](https://portal.azure.com) under **Browse** > **Virtual machines**, or by using hello following PowerShell commands:</span></span>

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="4981c-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4981c-183">Next steps</span></span>
<span data-ttu-id="4981c-184">toomanage nový virtuální počítač s prostředím Azure PowerShell najdete v části [Správa virtuálních počítačů pomocí Azure Resource Manageru a prostředí PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4981c-184">toomanage your new virtual machine with Azure PowerShell, see [Manage virtual machines using Azure Resource Manager and PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


