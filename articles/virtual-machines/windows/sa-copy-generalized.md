---
title: "Vytvoření image nespravované zobecněný virtuálního počítače v Azure | Microsoft Docs"
description: "Vytvoření image unmanged zobecněný virtuální počítač s Windows pomocí vytvářet více kopií virtuálního počítače v Azure."
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
ms.openlocfilehash: d7f4a9558175835eba9096e6845726f21c7459d3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-create-an-unmanaged-vm-image-from-an-azure-vm"></a><span data-ttu-id="2a4b6-103">Postup vytvoření nespravované image virtuálního počítače z virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="2a4b6-103">How to create an unmanaged VM image from an Azure VM</span></span>

<span data-ttu-id="2a4b6-104">Tento článek popisuje používání účtů úložiště.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-104">This article covers using storage accounts.</span></span> <span data-ttu-id="2a4b6-105">Doporučujeme místo účtu úložiště používat spravované disky a spravované bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-105">We recommend that you use managed disks and managed images instead of a storage account.</span></span> <span data-ttu-id="2a4b6-106">Další informace najdete v tématu [zaznamenat bitovou kopii spravované zobecněný virtuálního počítače v Azure](capture-image-resource.md).</span><span class="sxs-lookup"><span data-stu-id="2a4b6-106">For more information, see [Capture a managed image of a generalized VM in Azure](capture-image-resource.md).</span></span>

<span data-ttu-id="2a4b6-107">Tento článek ukazuje, jak pomocí prostředí Azure PowerShell k vytvoření bitové kopie zobecněný virtuální počítač Azure pomocí účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-107">This article shows you how to use Azure PowerShell to create an image of a generalized Azure VM using a storage account.</span></span> <span data-ttu-id="2a4b6-108">Potom můžete image vytvořit jiným virtuálním Počítačem.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-108">You can then use the image to create another VM.</span></span> <span data-ttu-id="2a4b6-109">Obrázek obsahuje disk operačního systému a datové disky, které jsou připojené k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-109">The image includes the OS disk and the data disks that are attached to the virtual machine.</span></span> <span data-ttu-id="2a4b6-110">Bitovou kopii neobsahuje virtuální síťové prostředky, takže budete muset nastavit tyto prostředky při vytváření nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-110">The image doesn't include the virtual network resources, so you need to set up those resources when you create the new VM.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="2a4b6-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2a4b6-111">Prerequisites</span></span>
<span data-ttu-id="2a4b6-112">Je potřeba mít prostředí Azure PowerShell verze 1.0.x nebo novější.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-112">You need to have Azure PowerShell version 1.0.x or newer installed.</span></span> <span data-ttu-id="2a4b6-113">Pokud jste ještě nenainstalovali prostředí PowerShell, přečtěte si [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) kroky instalace.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-113">If you haven't already installed PowerShell, read [How to install and configure Azure PowerShell](/powershell/azure/overview) for installation steps.</span></span>

## <a name="generalize-the-vm"></a><span data-ttu-id="2a4b6-114">Generalize virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="2a4b6-114">Generalize the VM</span></span> 
<span data-ttu-id="2a4b6-115">V této části se dozvíte, jak ke generalizaci virtuálního počítače Windows pro použití jako obrázek.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-115">This section shows you how to generalize your Windows virtual machine for use as an image.</span></span> <span data-ttu-id="2a4b6-116">Generalizací virtuálního počítače odebere všechny osobní informace o účtu, mimo jiné a připraví počítač, který se má použít jako obrázek.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-116">Generalizing a VM removes all your personal account information, among other things, and prepares the machine to be used as an image.</span></span> <span data-ttu-id="2a4b6-117">Podrobnosti o nástroji Sysprep najdete v tématu [postup použití nástroje Sysprep: Úvod](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="2a4b6-117">For details about Sysprep, see [How to Use Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>

<span data-ttu-id="2a4b6-118">Ujistěte se, že role serveru spuštěná na tomto počítači jsou podporovány nástrojem Sysprep.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-118">Make sure the server roles running on the machine are supported by Sysprep.</span></span> <span data-ttu-id="2a4b6-119">Další informace najdete v tématu [podpora nástroje Sysprep pro role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span><span class="sxs-lookup"><span data-stu-id="2a4b6-119">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2a4b6-120">Pokud nahráváte svůj disk VHD do Azure poprvé, zajistěte, aby byla [připravit virtuální počítač](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) před spuštěním nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-120">If you are uploading your VHD to Azure for the first time, make sure you have [prepared your VM](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before running Sysprep.</span></span> 
> 
> 

<span data-ttu-id="2a4b6-121">Můžete také generalize virtuálního počítače s Linuxem pomocí `sudo waagent -deprovision+user` a potom pomocí prostředí PowerShell pro zachycení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-121">You can also generalize a Linux VM using `sudo waagent -deprovision+user` and then use PowerShell to capture the VM.</span></span> <span data-ttu-id="2a4b6-122">Informace o zachycení virtuálního počítače pomocí rozhraní příkazového řádku najdete v tématu [generalize a zachycení virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure ](../linux/capture-image.md).</span><span class="sxs-lookup"><span data-stu-id="2a4b6-122">For information about using the CLI to capture a VM, see [How to generalize and capture a Linux virtual machine using the Azure CLI ](../linux/capture-image.md).</span></span>


1. <span data-ttu-id="2a4b6-123">Přihlaste se k virtuálnímu počítači Windows.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-123">Sign in to the Windows virtual machine.</span></span>
2. <span data-ttu-id="2a4b6-124">Otevřete okno příkazového řádku jako správce.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-124">Open the Command Prompt window as an administrator.</span></span> <span data-ttu-id="2a4b6-125">Změňte adresář na **%windir%\system32\sysprep**a poté spusťte `sysprep.exe`.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-125">Change the directory to **%windir%\system32\sysprep**, and then run `sysprep.exe`.</span></span>
3. <span data-ttu-id="2a4b6-126">V **nástroj pro přípravu systému** dialogové okno, vyberte **prostředí Out-of-Box zadejte systému (při prvním zapnutí)**a ujistěte se, že **generalizace** je zaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-126">In the **System Preparation Tool** dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that the **Generalize** check box is selected.</span></span>
4. <span data-ttu-id="2a4b6-127">V **možnosti vypnutí**, vyberte **vypnutí**.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-127">In **Shutdown Options**, select **Shutdown**.</span></span>
5. <span data-ttu-id="2a4b6-128">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-128">Click **OK**.</span></span>
   
    ![Spusťte nástroj Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. <span data-ttu-id="2a4b6-130">Po dokončení nástroj Sysprep vypne virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-130">When Sysprep completes, it shuts down the virtual machine.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="2a4b6-131">Virtuální počítač nerestartuje, až do dokončení odesílání virtuálního pevného disku do Azure nebo vytvoření bitové kopie z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-131">Do not restart the VM until you are done uploading the VHD to Azure or creating an image from the VM.</span></span> <span data-ttu-id="2a4b6-132">Pokud omylem získá restartování virtuálního počítače, spusťte nástroj Sysprep ke generalizaci ho znovu.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-132">If the VM accidentally gets restarted, run Sysprep to generalize it again.</span></span>
> 
> 

## <a name="log-in-to-azure-powershell"></a><span data-ttu-id="2a4b6-133">Přihlaste se k prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="2a4b6-133">Log in to Azure PowerShell</span></span>
1. <span data-ttu-id="2a4b6-134">Otevřete prostředí Azure PowerShell a přihlaste se k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-134">Open Azure PowerShell and sign in to your Azure account.</span></span>
   
    ```powershell
    Login-AzureRmAccount
    ```
   
    <span data-ttu-id="2a4b6-135">Automaticky otevírané okno otevře zadat přihlašovací údaje účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-135">A pop-up window opens for you to enter your Azure account credentials.</span></span>
2. <span data-ttu-id="2a4b6-136">Pořiďte si předplatné ID pro dostupných předplatných.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-136">Get the subscription IDs for your available subscriptions.</span></span>
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. <span data-ttu-id="2a4b6-137">Nastavit správné předplatné pomocí ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-137">Set the correct subscription using the subscription ID.</span></span>
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="deallocate-the-vm-and-set-the-state-to-generalized"></a><span data-ttu-id="2a4b6-138">Zrušit přidělení virtuálního počítače a nastavit stav, který má zobecněné</span><span class="sxs-lookup"><span data-stu-id="2a4b6-138">Deallocate the VM and set the state to generalized</span></span>
1. <span data-ttu-id="2a4b6-139">Deallocate prostředky virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-139">Deallocate the VM resources.</span></span>
   
    ```powershell
    Stop-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```
   
    <span data-ttu-id="2a4b6-140">*Stav* pro virtuální počítač ve službě Azure portal změní z **Zastaveno** k **zastaveném (nepřiřazeném)**.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-140">The *Status* for the VM in the Azure portal changes from **Stopped** to **Stopped (deallocated)**.</span></span>
2. <span data-ttu-id="2a4b6-141">Nastavte stav virtuálního počítače na **zobecněno**.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-141">Set the status of the virtual machine to **Generalized**.</span></span> 
   
    ```powershell
    Set-AzureRmVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```
3. <span data-ttu-id="2a4b6-142">Zkontrolujte stav virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-142">Check the status of the VM.</span></span> <span data-ttu-id="2a4b6-143">**OSState/zobecněn** části pro virtuální počítač by měl mít **DisplayStatus** nastavena na **zobecněný virtuální počítač**.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-143">The **OSState/generalized** section for the VM should have the **DisplayStatus** set to **VM generalized**.</span></span>  
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## <a name="create-the-image"></a><span data-ttu-id="2a4b6-144">Vytvoření bitové kopie</span><span class="sxs-lookup"><span data-stu-id="2a4b6-144">Create the image</span></span>

<span data-ttu-id="2a4b6-145">Vytvoření image nespravované virtuální počítač v cílový kontejner úložiště použití tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-145">Create an unmanaged virtual machine image in the destination storage container using this command.</span></span> <span data-ttu-id="2a4b6-146">Obrázek se vytvoří ve stejném účtu úložiště jako původní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-146">The image is created in the same storage account as the original virtual machine.</span></span> <span data-ttu-id="2a4b6-147">`-Path` Parametr uloží kopii šablona JSON pro zdrojový virtuální počítač do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-147">The `-Path` parameter saves a copy of the JSON template for the source VM to your local computer.</span></span> <span data-ttu-id="2a4b6-148">`-DestinationContainerName` Parametr je název kontejneru, který má být blokování obrázků.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-148">The `-DestinationContainerName` parameter is the name of the container that you want to hold your images.</span></span> <span data-ttu-id="2a4b6-149">Pokud kontejner neexistuje, vytvoří se pro vás.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-149">If the container doesn't exist, it is created for you.</span></span>
   
```powershell
Save-AzureRmVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
    -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
    -Path <C:\local\Filepath\Filename.json>
```
   
<span data-ttu-id="2a4b6-150">Můžete získat adresu URL bitové kopie ze souboru šablony JSON.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-150">You can get the URL of your image from the JSON file template.</span></span> <span data-ttu-id="2a4b6-151">Přejděte na **prostředky** > **storageProfile** > **osDisk** > **image**  >  **uri** části pro kompletní cesta bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-151">Go to the **resources** > **storageProfile** > **osDisk** > **image** > **uri** section for the complete path of your image.</span></span> <span data-ttu-id="2a4b6-152">Adresa URL obrázku, vypadá takto: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-152">The URL of the image looks like: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.</span></span>
   
<span data-ttu-id="2a4b6-153">Můžete si taky ověřit identifikátor URI na portálu.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-153">You can also verify the URI in the portal.</span></span> <span data-ttu-id="2a4b6-154">Obrázek se zkopíruje na kontejner s názvem **systému** ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-154">The image is copied to a container named **system** in your storage account.</span></span> 

## <a name="create-a-vm-from-the-image"></a><span data-ttu-id="2a4b6-155">Vytvořte virtuální počítač z bitové kopie</span><span class="sxs-lookup"><span data-stu-id="2a4b6-155">Create a VM from the image</span></span>

<span data-ttu-id="2a4b6-156">Nyní můžete vytvořit jeden nebo více virtuálních počítačů z nespravovaných bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-156">Now you can create one or more VMs from the unmanaged image.</span></span>

### <a name="set-the-uri-of-the-vhd"></a><span data-ttu-id="2a4b6-157">Nastavit identifikátor URI virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="2a4b6-157">Set the URI of the VHD</span></span>

<span data-ttu-id="2a4b6-158">Identifikátor URI pro VHD, který chcete používat je ve formátu: https://**můj_účet_úložiště**.blob.core.windows.net/**můj_kontejner**/**MyVhdName**VHD.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-158">The URI for the VHD to use is in the format: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**.vhd.</span></span> <span data-ttu-id="2a4b6-159">V tomto příkladu virtuální pevný disk s názvem **myVHD** je v účtu úložiště **můj_účet_úložiště** v kontejneru **můj_kontejner**.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-159">In this example the VHD named **myVHD** is in the storage account **mystorageaccount** in the container **mycontainer**.</span></span>

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


### <a name="create-a-virtual-network"></a><span data-ttu-id="2a4b6-160">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="2a4b6-160">Create a virtual network</span></span>
<span data-ttu-id="2a4b6-161">Vytvořit virtuální síť a podsíť [virtuální sítě](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2a4b6-161">Create the vNet and subnet of the [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="2a4b6-162">Vytvořte podsíť.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-162">Create the subnet.</span></span> <span data-ttu-id="2a4b6-163">Následující příklad vytvoří podsíť s názvem **mySubnet** ve skupině prostředků **myResourceGroup** s předponou adresy z **10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-163">The following sample creates a subnet named **mySubnet** in the resource group **myResourceGroup** with the address prefix of **10.0.0.0/24**.</span></span>  
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="2a4b6-164">Vytvořte virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-164">Create the virtual network.</span></span> <span data-ttu-id="2a4b6-165">Následující ukázka vytvoří virtuální síť s názvem **myVnet** v **západní USA** umístění s předponou adresy z **10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-165">The following sample creates a virtual network named **myVnet** in the **West US** location with the address prefix of **10.0.0.0/16**.</span></span>  
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-network-interface"></a><span data-ttu-id="2a4b6-166">Vytvoření veřejné IP adresy a síťového rozhraní</span><span class="sxs-lookup"><span data-stu-id="2a4b6-166">Create a public IP address and network interface</span></span>
<span data-ttu-id="2a4b6-167">Pokud chcete povolit komunikaci s virtuálním počítačem ve virtuální síti, budete potřebovat [veřejnou adresu IP](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) a síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-167">To enable communication with the virtual machine in the virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="2a4b6-168">Vytvoření veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-168">Create a public IP address.</span></span> <span data-ttu-id="2a4b6-169">Tento příklad vytvoří veřejnou IP adresu s názvem **myPip**.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-169">This example creates a public IP address named **myPip**.</span></span> 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="2a4b6-170">Vytvořit síťovou kartu.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-170">Create the NIC.</span></span> <span data-ttu-id="2a4b6-171">Tento příklad vytvoří síťový adaptér s názvem **myNic**.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-171">This example creates a NIC named **myNic**.</span></span> 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-the-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="2a4b6-172">Vytvořte skupinu zabezpečení sítě a pravidlo protokolu RDP</span><span class="sxs-lookup"><span data-stu-id="2a4b6-172">Create the network security group and an RDP rule</span></span>
<span data-ttu-id="2a4b6-173">Abyste mohli přihlásit k virtuálnímu počítači pomocí protokolu RDP, musíte mít pravidlo zabezpečení, která umožňuje přístup RDP na portu 3389.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-173">To be able to log in to your VM using RDP, you need to have a security rule that allows RDP access on port 3389.</span></span> 

<span data-ttu-id="2a4b6-174">Tento příklad vytvoří skupinu NSG s názvem **myNsg** obsahující pravidlo názvem **myRdpRule** přes port 3389 umožňuje provoz protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-174">This example creates an NSG named **myNsg** that contains a rule called **myRdpRule** that allows RDP traffic over port 3389.</span></span> <span data-ttu-id="2a4b6-175">Další informace o skupinách Nsg najdete v tématu [otevřít porty pro virtuální počítač v Azure pomocí prostředí PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2a4b6-175">For more information about NSGs, see [Opening ports to a VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


### <a name="create-a-variable-for-the-virtual-network"></a><span data-ttu-id="2a4b6-176">Vytvořte proměnnou pro virtuální síť</span><span class="sxs-lookup"><span data-stu-id="2a4b6-176">Create a variable for the virtual network</span></span>
<span data-ttu-id="2a4b6-177">Vytvořte proměnnou pro dokončené virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-177">Create a variable for the completed virtual network.</span></span> 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

### <a name="create-the-vm"></a><span data-ttu-id="2a4b6-178">Vytvořte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-178">Create the VM</span></span>
<span data-ttu-id="2a4b6-179">Následující PowerShell dokončí konfigurací virtuálních počítačů a nespravované image se používá jako zdroj pro novou instalaci.</span><span class="sxs-lookup"><span data-stu-id="2a4b6-179">The following PowerShell completes the virtual machine configurations and uses unmanaged image as the source for the new installation.</span></span>

</br>

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

### <a name="verify-that-the-vm-was-created"></a><span data-ttu-id="2a4b6-180">Zkontrolujte, zda byl vytvořen virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="2a4b6-180">Verify that the VM was created</span></span>
<span data-ttu-id="2a4b6-181">Po dokončení byste měli vidět nově vytvořený virtuální počítač v [portál Azure](https://portal.azure.com) pod **Procházet** > **virtuální počítače**, nebo pomocí následujících příkazů prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="2a4b6-181">When complete, you should see the newly created VM in the [Azure portal](https://portal.azure.com) under **Browse** > **Virtual machines**, or by using the following PowerShell commands:</span></span>

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="2a4b6-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2a4b6-182">Next steps</span></span>
<span data-ttu-id="2a4b6-183">Ke správě nového virtuálního počítače v prostředí Azure PowerShell najdete [Správa virtuálních počítačů pomocí Azure Resource Manageru a prostředí PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2a4b6-183">To manage your new virtual machine with Azure PowerShell, see [Manage virtual machines using Azure Resource Manager and PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


