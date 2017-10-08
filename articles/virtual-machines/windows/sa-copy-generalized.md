---
title: "aaaCreate nespravované image zobecněný virtuální počítač v Azure | Microsoft Docs"
description: "Vytvořte image unmanged zobecněný toocreate toouse virtuální počítač s Windows více kopií virtuálního počítače v Azure."
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
ms.date: 05/23/2017
ms.author: cynthn
ms.openlocfilehash: b8a044d20313efa6dd9b9757e61154f922134476
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-an-unmanaged-vm-image-from-an-azure-vm"></a><span data-ttu-id="dd909-103">Jak obrázek toocreate nespravované virtuálního počítače z virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="dd909-103">How toocreate an unmanaged VM image from an Azure VM</span></span>

<span data-ttu-id="dd909-104">Tento článek popisuje používání účtů úložiště.</span><span class="sxs-lookup"><span data-stu-id="dd909-104">This article covers using storage accounts.</span></span> <span data-ttu-id="dd909-105">Doporučujeme místo účtu úložiště používat spravované disky a spravované bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="dd909-105">We recommend that you use managed disks and managed images instead of a storage account.</span></span> <span data-ttu-id="dd909-106">Další informace najdete v tématu [zaznamenat bitovou kopii spravované zobecněný virtuálního počítače v Azure](capture-image-resource.md).</span><span class="sxs-lookup"><span data-stu-id="dd909-106">For more information, see [Capture a managed image of a generalized VM in Azure](capture-image-resource.md).</span></span>

<span data-ttu-id="dd909-107">Tento článek ukazuje, jak toouse prostředí Azure PowerShell toocreate image zobecněný virtuálního počítače Azure pomocí účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="dd909-107">This article shows you how toouse Azure PowerShell toocreate an image of a generalized Azure VM using a storage account.</span></span> <span data-ttu-id="dd909-108">Potom můžete hello image toocreate jiným virtuálním Počítačem.</span><span class="sxs-lookup"><span data-stu-id="dd909-108">You can then use hello image toocreate another VM.</span></span> <span data-ttu-id="dd909-109">Hello image obsahuje hello disk operačního systému a hello datových disků, které jsou připojené toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="dd909-109">hello image includes hello OS disk and hello data disks that are attached toohello virtual machine.</span></span> <span data-ttu-id="dd909-110">Obrázek Hello neobsahuje hello virtuální síťové prostředky, proto musíte tooset si tyto prostředky při vytváření nového virtuálního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="dd909-110">hello image doesn't include hello virtual network resources, so you need tooset up those resources when you create hello new VM.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="dd909-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="dd909-111">Prerequisites</span></span>
<span data-ttu-id="dd909-112">Je třeba toohave prostředí Azure PowerShell verze 1.0.x nebo novější.</span><span class="sxs-lookup"><span data-stu-id="dd909-112">You need toohave Azure PowerShell version 1.0.x or newer installed.</span></span> <span data-ttu-id="dd909-113">Pokud jste ještě nenainstalovali prostředí PowerShell, přečtěte si [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) kroky instalace.</span><span class="sxs-lookup"><span data-stu-id="dd909-113">If you haven't already installed PowerShell, read [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for installation steps.</span></span>

## <a name="generalize-hello-vm"></a><span data-ttu-id="dd909-114">Generalize hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="dd909-114">Generalize hello VM</span></span> 
<span data-ttu-id="dd909-115">V této části se dozvíte, jak toogeneralize virtuálního počítače Windows pro použití jako obrázek.</span><span class="sxs-lookup"><span data-stu-id="dd909-115">This section shows you how toogeneralize your Windows virtual machine for use as an image.</span></span> <span data-ttu-id="dd909-116">Generalizací virtuálního počítače odebere všechny osobní informace o účtu, mimo jiné a připraví toobe počítač hello použít jako obrázek.</span><span class="sxs-lookup"><span data-stu-id="dd909-116">Generalizing a VM removes all your personal account information, among other things, and prepares hello machine toobe used as an image.</span></span> <span data-ttu-id="dd909-117">Podrobnosti o nástroji Sysprep najdete v tématu [jak tooUse nástroje Sysprep: Úvod](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="dd909-117">For details about Sysprep, see [How tooUse Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>

<span data-ttu-id="dd909-118">Ujistěte se, že role serveru hello spuštěné na počítači hello jsou podporovány nástrojem Sysprep.</span><span class="sxs-lookup"><span data-stu-id="dd909-118">Make sure hello server roles running on hello machine are supported by Sysprep.</span></span> <span data-ttu-id="dd909-119">Další informace najdete v tématu [podpora nástroje Sysprep pro role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span><span class="sxs-lookup"><span data-stu-id="dd909-119">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dd909-120">Pokud nahráváte vaší tooAzure virtuálního pevného disku pro hello poprvé, zajistěte, aby byla [připravit virtuální počítač](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) před spuštěním nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="dd909-120">If you are uploading your VHD tooAzure for hello first time, make sure you have [prepared your VM](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before running Sysprep.</span></span> 
> 
> 

<span data-ttu-id="dd909-121">Můžete také generalize virtuálního počítače s Linuxem pomocí `sudo waagent -deprovision+user` a potom pomocí prostředí PowerShell toocapture hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="dd909-121">You can also generalize a Linux VM using `sudo waagent -deprovision+user` and then use PowerShell toocapture hello VM.</span></span> <span data-ttu-id="dd909-122">Informace o používání rozhraní příkazového řádku toocapture hello virtuálního počítače najdete v tématu [jak hello toogeneralize a zachycení virtuálního počítače Linux pomocí příkazového řádku Azure CLI ](../linux/capture-image.md).</span><span class="sxs-lookup"><span data-stu-id="dd909-122">For information about using hello CLI toocapture a VM, see [How toogeneralize and capture a Linux virtual machine using hello Azure CLI ](../linux/capture-image.md).</span></span>


1. <span data-ttu-id="dd909-123">Přihlaste se toohello virtuálního počítače Windows.</span><span class="sxs-lookup"><span data-stu-id="dd909-123">Sign in toohello Windows virtual machine.</span></span>
2. <span data-ttu-id="dd909-124">Otevřete okno příkazového řádku hello jako správce.</span><span class="sxs-lookup"><span data-stu-id="dd909-124">Open hello Command Prompt window as an administrator.</span></span> <span data-ttu-id="dd909-125">Změnit adresář, hello příliš**%windir%\system32\sysprep**a poté spusťte `sysprep.exe`.</span><span class="sxs-lookup"><span data-stu-id="dd909-125">Change hello directory too**%windir%\system32\sysprep**, and then run `sysprep.exe`.</span></span>
3. <span data-ttu-id="dd909-126">V hello **nástroj pro přípravu systému** dialogové okno, vyberte **prostředí Out-of-Box zadejte systému (při prvním zapnutí)**a ujistěte se, že hello **generalizace** je zaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="dd909-126">In hello **System Preparation Tool** dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that hello **Generalize** check box is selected.</span></span>
4. <span data-ttu-id="dd909-127">V **možnosti vypnutí**, vyberte **vypnutí**.</span><span class="sxs-lookup"><span data-stu-id="dd909-127">In **Shutdown Options**, select **Shutdown**.</span></span>
5. <span data-ttu-id="dd909-128">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="dd909-128">Click **OK**.</span></span>
   
    ![Spusťte nástroj Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. <span data-ttu-id="dd909-130">Po dokončení nástroj Sysprep vypne hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="dd909-130">When Sysprep completes, it shuts down hello virtual machine.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="dd909-131">Nerestartovat hello virtuálních počítačů, dokud jste done odesílání tooAzure hello virtuálního pevného disku nebo vytvoření bitové kopie z hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="dd909-131">Do not restart hello VM until you are done uploading hello VHD tooAzure or creating an image from hello VM.</span></span> <span data-ttu-id="dd909-132">Pokud omylem získá restartována hello virtuálního počítače, spusťte nástroj Sysprep toogeneralize ho znovu.</span><span class="sxs-lookup"><span data-stu-id="dd909-132">If hello VM accidentally gets restarted, run Sysprep toogeneralize it again.</span></span>
> 
> 

## <a name="log-in-tooazure-powershell"></a><span data-ttu-id="dd909-133">Přihlaste se tooAzure prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="dd909-133">Log in tooAzure PowerShell</span></span>
1. <span data-ttu-id="dd909-134">Otevřete prostředí Azure PowerShell a přihlaste se tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="dd909-134">Open Azure PowerShell and sign in tooyour Azure account.</span></span>
   
    ```powershell
    Login-AzureRmAccount
    ```
   
    <span data-ttu-id="dd909-135">Automaticky otevírané okno otevře pro tooenter jste přihlašovací údaje účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="dd909-135">A pop-up window opens for you tooenter your Azure account credentials.</span></span>
2. <span data-ttu-id="dd909-136">Pořiďte si předplatné hello ID pro dostupných předplatných.</span><span class="sxs-lookup"><span data-stu-id="dd909-136">Get hello subscription IDs for your available subscriptions.</span></span>
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. <span data-ttu-id="dd909-137">Nastavit správné předplatné hello pomocí ID hello předplatného.</span><span class="sxs-lookup"><span data-stu-id="dd909-137">Set hello correct subscription using hello subscription ID.</span></span>
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="deallocate-hello-vm-and-set-hello-state-toogeneralized"></a><span data-ttu-id="dd909-138">Deallocate hello virtuálních počítačů a nastavte toogeneralized stavu hello</span><span class="sxs-lookup"><span data-stu-id="dd909-138">Deallocate hello VM and set hello state toogeneralized</span></span>
1. <span data-ttu-id="dd909-139">Deallocate hello prostředky virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="dd909-139">Deallocate hello VM resources.</span></span>
   
    ```powershell
    Stop-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```
   
    <span data-ttu-id="dd909-140">Hello *stav* pro hello virtuálních počítačů v hello Azure portal změní z **Zastaveno** příliš**zastaveném (nepřiřazeném)**.</span><span class="sxs-lookup"><span data-stu-id="dd909-140">hello *Status* for hello VM in hello Azure portal changes from **Stopped** too**Stopped (deallocated)**.</span></span>
2. <span data-ttu-id="dd909-141">Nastavte stav hello hello virtuálního počítače příliš**zobecněno**.</span><span class="sxs-lookup"><span data-stu-id="dd909-141">Set hello status of hello virtual machine too**Generalized**.</span></span> 
   
    ```powershell
    Set-AzureRmVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```
3. <span data-ttu-id="dd909-142">Zkontrolujte stav hello hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="dd909-142">Check hello status of hello VM.</span></span> <span data-ttu-id="dd909-143">Hello **OSState/zobecněn** části pro hello virtuální počítač by měl mít hello **DisplayStatus** nastavit příliš**zobecněný virtuální počítač**.</span><span class="sxs-lookup"><span data-stu-id="dd909-143">hello **OSState/generalized** section for hello VM should have hello **DisplayStatus** set too**VM generalized**.</span></span>  
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## <a name="create-hello-image"></a><span data-ttu-id="dd909-144">Vytvoření bitové kopie hello</span><span class="sxs-lookup"><span data-stu-id="dd909-144">Create hello image</span></span>

<span data-ttu-id="dd909-145">Vytvoření image nespravované virtuální počítač v hello cílový kontejner úložiště použití tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="dd909-145">Create an unmanaged virtual machine image in hello destination storage container using this command.</span></span> <span data-ttu-id="dd909-146">Hello bitové kopie je vytvořen v hello stejný účet úložiště jako hello původní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="dd909-146">hello image is created in hello same storage account as hello original virtual machine.</span></span> <span data-ttu-id="dd909-147">Hello `-Path` parametr uloží kopii hello šablona JSON pro hello zdrojový virtuální počítač tooyour místní počítač.</span><span class="sxs-lookup"><span data-stu-id="dd909-147">hello `-Path` parameter saves a copy of hello JSON template for hello source VM tooyour local computer.</span></span> <span data-ttu-id="dd909-148">Hello `-DestinationContainerName` parametr je hello název kontejneru hello, které chcete toohold obrázků.</span><span class="sxs-lookup"><span data-stu-id="dd909-148">hello `-DestinationContainerName` parameter is hello name of hello container that you want toohold your images.</span></span> <span data-ttu-id="dd909-149">Pokud hello kontejner neexistuje, vytvoří se pro vás.</span><span class="sxs-lookup"><span data-stu-id="dd909-149">If hello container doesn't exist, it is created for you.</span></span>
   
```powershell
Save-AzureRmVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
    -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
    -Path <C:\local\Filepath\Filename.json>
```
   
<span data-ttu-id="dd909-150">Adresa URL hello vaší bitové kopie můžete získat z hello šablona souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="dd909-150">You can get hello URL of your image from hello JSON file template.</span></span> <span data-ttu-id="dd909-151">Přejděte toohello **prostředky** > **storageProfile** > **osDisk** > **image**  >  **uri** části hello kompletní cesta bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="dd909-151">Go toohello **resources** > **storageProfile** > **osDisk** > **image** > **uri** section for hello complete path of your image.</span></span> <span data-ttu-id="dd909-152">vypadá Hello adresa URL obrázku hello: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.</span><span class="sxs-lookup"><span data-stu-id="dd909-152">hello URL of hello image looks like: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.</span></span>
   
<span data-ttu-id="dd909-153">Můžete si taky ověřit hello URI hello portálu.</span><span class="sxs-lookup"><span data-stu-id="dd909-153">You can also verify hello URI in hello portal.</span></span> <span data-ttu-id="dd909-154">Obrázek Hello je zkopírovaný tooa kontejner s názvem **systému** ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="dd909-154">hello image is copied tooa container named **system** in your storage account.</span></span> 

## <a name="create-a-vm-from-hello-image"></a><span data-ttu-id="dd909-155">Vytvoření virtuálního počítače z hello image</span><span class="sxs-lookup"><span data-stu-id="dd909-155">Create a VM from hello image</span></span>

<span data-ttu-id="dd909-156">Nyní můžete vytvořit jeden nebo více virtuálních počítačů z nespravovaných image hello.</span><span class="sxs-lookup"><span data-stu-id="dd909-156">Now you can create one or more VMs from hello unmanaged image.</span></span>

### <a name="set-hello-uri-of-hello-vhd"></a><span data-ttu-id="dd909-157">Nastavit hello URI hello virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="dd909-157">Set hello URI of hello VHD</span></span>

<span data-ttu-id="dd909-158">Hello identifikátor URI pro toouse hello virtuální pevný disk je ve formátu hello: https://**můj_účet_úložiště**.blob.core.windows.net/**můj_kontejner**/**MyVhdName**VHD.</span><span class="sxs-lookup"><span data-stu-id="dd909-158">hello URI for hello VHD toouse is in hello format: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**.vhd.</span></span> <span data-ttu-id="dd909-159">V tomto příkladu hello virtuálního pevného disku s názvem **myVHD** v účtu úložiště hello **můj_účet_úložiště** v kontejneru hello **můj_kontejner**.</span><span class="sxs-lookup"><span data-stu-id="dd909-159">In this example hello VHD named **myVHD** is in hello storage account **mystorageaccount** in hello container **mycontainer**.</span></span>

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


### <a name="create-a-virtual-network"></a><span data-ttu-id="dd909-160">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="dd909-160">Create a virtual network</span></span>
<span data-ttu-id="dd909-161">Vytvoření hello virtuální síť a podsíť hello [virtuální sítě](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="dd909-161">Create hello vNet and subnet of hello [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="dd909-162">Vytvořte podsíť hello.</span><span class="sxs-lookup"><span data-stu-id="dd909-162">Create hello subnet.</span></span> <span data-ttu-id="dd909-163">Hello následující ukázka vytvoří podsíť s názvem **mySubnet** ve skupině prostředků hello **myResourceGroup** s předponou adresy hello z **10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="dd909-163">hello following sample creates a subnet named **mySubnet** in hello resource group **myResourceGroup** with hello address prefix of **10.0.0.0/24**.</span></span>  
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="dd909-164">Vytvoření virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="dd909-164">Create hello virtual network.</span></span> <span data-ttu-id="dd909-165">Hello následující ukázka vytvoří virtuální síť s názvem **myVnet** v hello **západní USA** umístění s předponu adresy hello **10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="dd909-165">hello following sample creates a virtual network named **myVnet** in hello **West US** location with hello address prefix of **10.0.0.0/16**.</span></span>  
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-network-interface"></a><span data-ttu-id="dd909-166">Vytvoření veřejné IP adresy a síťového rozhraní</span><span class="sxs-lookup"><span data-stu-id="dd909-166">Create a public IP address and network interface</span></span>
<span data-ttu-id="dd909-167">tooenable komunikaci s hello virtuálním počítačem ve virtuální síti hello, musíte [veřejnou IP adresu](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) a síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="dd909-167">tooenable communication with hello virtual machine in hello virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="dd909-168">Vytvoření veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="dd909-168">Create a public IP address.</span></span> <span data-ttu-id="dd909-169">Tento příklad vytvoří veřejnou IP adresu s názvem **myPip**.</span><span class="sxs-lookup"><span data-stu-id="dd909-169">This example creates a public IP address named **myPip**.</span></span> 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="dd909-170">Vytvoření hello síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="dd909-170">Create hello NIC.</span></span> <span data-ttu-id="dd909-171">Tento příklad vytvoří síťový adaptér s názvem **myNic**.</span><span class="sxs-lookup"><span data-stu-id="dd909-171">This example creates a NIC named **myNic**.</span></span> 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-hello-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="dd909-172">Vytvořte skupinu zabezpečení sítě hello a pravidlo protokolu RDP</span><span class="sxs-lookup"><span data-stu-id="dd909-172">Create hello network security group and an RDP rule</span></span>
<span data-ttu-id="dd909-173">možnost toolog toobe v tooyour virtuálního počítače pomocí protokolu RDP, je nutné toohave pravidlo zabezpečení, která umožňuje přístup RDP na portu 3389.</span><span class="sxs-lookup"><span data-stu-id="dd909-173">toobe able toolog in tooyour VM using RDP, you need toohave a security rule that allows RDP access on port 3389.</span></span> 

<span data-ttu-id="dd909-174">Tento příklad vytvoří skupinu NSG s názvem **myNsg** obsahující pravidlo názvem **myRdpRule** přes port 3389 umožňuje provoz protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="dd909-174">This example creates an NSG named **myNsg** that contains a rule called **myRdpRule** that allows RDP traffic over port 3389.</span></span> <span data-ttu-id="dd909-175">Další informace o skupinách Nsg najdete v tématu [otevřít porty tooa virtuálních počítačů v Azure pomocí prostředí PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="dd909-175">For more information about NSGs, see [Opening ports tooa VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


### <a name="create-a-variable-for-hello-virtual-network"></a><span data-ttu-id="dd909-176">Vytvořte proměnnou pro virtuální síť hello</span><span class="sxs-lookup"><span data-stu-id="dd909-176">Create a variable for hello virtual network</span></span>
<span data-ttu-id="dd909-177">Vytvořte proměnnou pro virtuální síť hello byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="dd909-177">Create a variable for hello completed virtual network.</span></span> 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

### <a name="create-hello-vm"></a><span data-ttu-id="dd909-178">Vytvoření hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="dd909-178">Create hello VM</span></span>
<span data-ttu-id="dd909-179">Hello následující PowerShell dokončí hello konfigurace virtuálních počítačů a nespravované image se používá jako hello zdroj pro novou instalaci hello.</span><span class="sxs-lookup"><span data-stu-id="dd909-179">hello following PowerShell completes hello virtual machine configurations and uses unmanaged image as hello source for hello new installation.</span></span>

</br>

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

### <a name="verify-that-hello-vm-was-created"></a><span data-ttu-id="dd909-180">Ověřte, že hello, ke které byl virtuální počítač vytvořen</span><span class="sxs-lookup"><span data-stu-id="dd909-180">Verify that hello VM was created</span></span>
<span data-ttu-id="dd909-181">Po dokončení byste měli vidět hello nově vytvořený virtuální počítač v hello [portál Azure](https://portal.azure.com) pod **Procházet** > **virtuální počítače**, nebo pomocí následujících hello Příkazy prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="dd909-181">When complete, you should see hello newly created VM in hello [Azure portal](https://portal.azure.com) under **Browse** > **Virtual machines**, or by using hello following PowerShell commands:</span></span>

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="dd909-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dd909-182">Next steps</span></span>
<span data-ttu-id="dd909-183">toomanage nový virtuální počítač s prostředím Azure PowerShell najdete v části [Správa virtuálních počítačů pomocí Azure Resource Manageru a prostředí PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="dd909-183">toomanage your new virtual machine with Azure PowerShell, see [Manage virtual machines using Azure Resource Manager and PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


