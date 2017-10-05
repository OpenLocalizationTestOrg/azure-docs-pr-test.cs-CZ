---
title: "Vytvoření virtuálního počítače ze spravovaných image virtuálního počítače v Azure | Microsoft Docs"
description: "Vytvoření virtuálního počítače Windows z zobecněný spravované image virtuálního počítače pomocí prostředí Azure PowerShell v modelu nasazení Resource Manager."
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
ms.date: 05/22/2017
ms.author: cynthn
ms.openlocfilehash: 2bb2d66271178a64ec0f4642e46b23f5618a56d9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-vm-from-a-managed-image"></a><span data-ttu-id="e13bb-103">Vytvoření virtuálního počítače ze spravovaných bitové kopie</span><span class="sxs-lookup"><span data-stu-id="e13bb-103">Create a VM from a managed image</span></span>

<span data-ttu-id="e13bb-104">Můžete vytvořit víc virtuálních počítačů ze spravovaných image virtuálního počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="e13bb-104">You can create multiple VMs from a managed VM image in Azure.</span></span> <span data-ttu-id="e13bb-105">Spravované image virtuálního počítače obsahuje informace potřebné k vytvoření virtuálního počítače, včetně disků operačního systému a data.</span><span class="sxs-lookup"><span data-stu-id="e13bb-105">A managed VM image contains the information necessary to create a VM, including the OS and data disks.</span></span> <span data-ttu-id="e13bb-106">Virtuální pevné disky, které tvoří bitovou kopii, včetně disků operačního systému a všech datových disků, jsou uloženy jako spravované disky.</span><span class="sxs-lookup"><span data-stu-id="e13bb-106">The VHDs that make up the image, including both the OS disks and any data disks, are stored as managed disks.</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="e13bb-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e13bb-107">Prerequisites</span></span>

<span data-ttu-id="e13bb-108">Musíte mít již [vytvořit spravované image virtuálního počítače](capture-image-resource.md) použít pro vytvoření nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e13bb-108">You need to have already [created a managed VM image](capture-image-resource.md) to use for creating the new VM.</span></span> 

<span data-ttu-id="e13bb-109">Ujistěte se, že máte nejnovější verzi modulů AzureRM.Compute a AzureRM.Network prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e13bb-109">Make sure that you have the latest versions of the AzureRM.Compute and AzureRM.Network PowerShell modules.</span></span> <span data-ttu-id="e13bb-110">Otevřete příkazovém řádku prostředí PowerShell jako správce a spusťte následující příkaz k instalaci.</span><span class="sxs-lookup"><span data-stu-id="e13bb-110">Open a PowerShell prompt as an Administrator and run the following command to install them.</span></span>

```powershell
Install-Module AzureRM.Compute,AzureRM.Network
```
<span data-ttu-id="e13bb-111">Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e13bb-111">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>



## <a name="collect-information-about-the-image"></a><span data-ttu-id="e13bb-112">Shromažďovat informace o bitové kopii</span><span class="sxs-lookup"><span data-stu-id="e13bb-112">Collect information about the image</span></span>

<span data-ttu-id="e13bb-113">Nejprve musíme shromažďovat základní informace o bitové kopii a vytvořte proměnnou pro bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="e13bb-113">First we need to gather basic information about the image and create a variable for the image.</span></span> <span data-ttu-id="e13bb-114">Tento příklad používá spravované image virtuálního počítače s názvem **myImage** který se v **myResourceGroup** skupinu prostředků **– Západ střední USA** umístění.</span><span class="sxs-lookup"><span data-stu-id="e13bb-114">This example uses a managed VM image named **myImage** that is in the **myResourceGroup** resource group in the **West Central US** location.</span></span> 

```powershell
$rgName = "myResourceGroup"
$location = "West Central US"
$imageName = "myImage"
$image = Get-AzureRMImage -ImageName $imageName -ResourceGroupName $rgName
```

## <a name="create-a-virtual-network"></a><span data-ttu-id="e13bb-115">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="e13bb-115">Create a virtual network</span></span>
<span data-ttu-id="e13bb-116">Vytvořit virtuální síť a podsíť [virtuální sítě](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e13bb-116">Create the vNet and subnet of the [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="e13bb-117">Vytvořte podsíť.</span><span class="sxs-lookup"><span data-stu-id="e13bb-117">Create the subnet.</span></span> <span data-ttu-id="e13bb-118">Tento příklad vytvoří podsíť s názvem **mySubnet** s předponou adresy z **10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="e13bb-118">This example creates a subnet named **mySubnet** with the address prefix of **10.0.0.0/24**.</span></span>  
   
    ```powershell
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="e13bb-119">Vytvořte virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="e13bb-119">Create the virtual network.</span></span> <span data-ttu-id="e13bb-120">Tento příklad vytvoří virtuální síť s názvem **myVnet** s předponou adresy z **10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="e13bb-120">This example creates a virtual network named **myVnet** with the address prefix of **10.0.0.0/16**.</span></span>  
   
    ```powershell
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-network-interface"></a><span data-ttu-id="e13bb-121">Vytvoření veřejné IP adresy a síťového rozhraní</span><span class="sxs-lookup"><span data-stu-id="e13bb-121">Create a public IP address and network interface</span></span>

<span data-ttu-id="e13bb-122">Pokud chcete povolit komunikaci s virtuálním počítačem ve virtuální síti, budete potřebovat [veřejnou adresu IP](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) a síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e13bb-122">To enable communication with the virtual machine in the virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="e13bb-123">Vytvoření veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="e13bb-123">Create a public IP address.</span></span> <span data-ttu-id="e13bb-124">Tento příklad vytvoří veřejnou IP adresu s názvem **myPip**.</span><span class="sxs-lookup"><span data-stu-id="e13bb-124">This example creates a public IP address named **myPip**.</span></span> 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="e13bb-125">Vytvořit síťovou kartu.</span><span class="sxs-lookup"><span data-stu-id="e13bb-125">Create the NIC.</span></span> <span data-ttu-id="e13bb-126">Tento příklad vytvoří síťový adaptér s názvem **myNic**.</span><span class="sxs-lookup"><span data-stu-id="e13bb-126">This example creates a NIC named **myNic**.</span></span> 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="e13bb-127">Vytvořte skupinu zabezpečení sítě a pravidlo protokolu RDP</span><span class="sxs-lookup"><span data-stu-id="e13bb-127">Create the network security group and an RDP rule</span></span>

<span data-ttu-id="e13bb-128">Abyste mohli přihlásit k virtuálnímu počítači pomocí protokolu RDP, musíte mít pravidla zabezpečení sítě (NSG), který umožňuje přístup RDP na portu 3389.</span><span class="sxs-lookup"><span data-stu-id="e13bb-128">To be able to log in to your VM using RDP, you need to have a network security rule (NSG) that allows RDP access on port 3389.</span></span> 

<span data-ttu-id="e13bb-129">Tento příklad vytvoří skupinu NSG s názvem **myNsg** obsahující pravidlo názvem **myRdpRule** přes port 3389 umožňuje provoz protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="e13bb-129">This example creates an NSG named **myNsg** that contains a rule called **myRdpRule** that allows RDP traffic over port 3389.</span></span> <span data-ttu-id="e13bb-130">Další informace o skupinách Nsg najdete v tématu [otevřít porty pro virtuální počítač v Azure pomocí prostředí PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e13bb-130">For more information about NSGs, see [Opening ports to a VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

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


## <a name="create-a-variable-for-the-virtual-network"></a><span data-ttu-id="e13bb-131">Vytvořte proměnnou pro virtuální síť</span><span class="sxs-lookup"><span data-stu-id="e13bb-131">Create a variable for the virtual network</span></span>

<span data-ttu-id="e13bb-132">Vytvořte proměnnou pro dokončené virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="e13bb-132">Create a variable for the completed virtual network.</span></span> 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

## <a name="get-the-credentials-for-the-vm"></a><span data-ttu-id="e13bb-133">Získání přihlašovacích údajů pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e13bb-133">Get the credentials for the VM</span></span>

<span data-ttu-id="e13bb-134">Následující rutiny otevře okno, kde bude zadejte nové uživatelské jméno a heslo pro použití jako účet místního správce pro vzdálený přístup k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="e13bb-134">The following cmdlet will open a window where you will enter a new user name and password to use as the local administrator account for remotely accessing the VM.</span></span> 

```powershell
$cred = Get-Credential
```

## <a name="set-variables-for-the-vm-name-computer-name-and-the-size-of-the-vm"></a><span data-ttu-id="e13bb-135">Nastavení proměnných pro název virtuálního počítače, název počítače a velikosti virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="e13bb-135">Set variables for the VM name, computer name and the size of the VM</span></span>

1. <span data-ttu-id="e13bb-136">Vytváření proměnných pro název virtuálního počítače a název počítače.</span><span class="sxs-lookup"><span data-stu-id="e13bb-136">Create variables for the VM name and computer name.</span></span> <span data-ttu-id="e13bb-137">Tento příklad nastaví název virtuálního počítače jako **Můjvp** a název počítače jako **tento počítač**.</span><span class="sxs-lookup"><span data-stu-id="e13bb-137">This example sets the VM name as **myVM** and the computer name as **myComputer**.</span></span>

    ```powershell
    $vmName = "myVM"
    $computerName = "myComputer"
    ```
2. <span data-ttu-id="e13bb-138">Nastavení velikosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e13bb-138">Set the size of the virtual machine.</span></span> <span data-ttu-id="e13bb-139">Tento příklad vytvoří **Standard_DS1_v2** velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e13bb-139">This example creates **Standard_DS1_v2** sized VM.</span></span> <span data-ttu-id="e13bb-140">Najdete v článku [velikosti virtuálních počítačů](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) Další informace naleznete v dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="e13bb-140">See the [VM sizes](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) documentation for more information.</span></span>

    ```powershell
    $vmSize = "Standard_DS1_v2"
    ```

3. <span data-ttu-id="e13bb-141">Přidejte název virtuálního počítače a velikost ke konfiguraci virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e13bb-141">Add the VM name and size to the VM configuration.</span></span>

```powershell
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

## <a name="set-the-vm-image-as-source-image-for-the-new-vm"></a><span data-ttu-id="e13bb-142">Nastavit image virtuálního počítače jako zdroj bitové kopie pro nový virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="e13bb-142">Set the VM image as source image for the new VM</span></span>

<span data-ttu-id="e13bb-143">Nastavte zdrojové bitové kopie pomocí ID spravované image virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e13bb-143">Set the source image using the ID of the managed VM image.</span></span>

```powershell
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## <a name="set-the-os-configuration-and-add-the-nic"></a><span data-ttu-id="e13bb-144">Nastavení konfigurace operačního systému a přidejte na síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="e13bb-144">Set the OS configuration and add the NIC.</span></span>

<span data-ttu-id="e13bb-145">Zadejte typ úložiště (PremiumLRS nebo StandardLRS) a velikost disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="e13bb-145">Enter the storage type (PremiumLRS or StandardLRS) and the size of the OS disk.</span></span> <span data-ttu-id="e13bb-146">Tento příklad nastaví typ účtu **PremiumLRS**, velikost disku na **128 GB** a ukládání do mezipaměti na disku ke **ReadWrite**.</span><span class="sxs-lookup"><span data-stu-id="e13bb-146">This example sets the account type to **PremiumLRS**, the disk size to **128 GB** and disk caching to **ReadWrite**.</span></span>

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm  -StorageAccountType PremiumLRS -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## <a name="create-the-vm"></a><span data-ttu-id="e13bb-147">Vytvořte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e13bb-147">Create the VM</span></span>

<span data-ttu-id="e13bb-148">Vytvoření nového virtuálního počítače pomocí konfigurace, který jsme vytvořen a uložen v **$vm** proměnné.</span><span class="sxs-lookup"><span data-stu-id="e13bb-148">Create the new Vm using the configuration that we have built and stored in the **$vm** variable.</span></span>

```powershell
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

## <a name="verify-that-the-vm-was-created"></a><span data-ttu-id="e13bb-149">Zkontrolujte, zda byl vytvořen virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="e13bb-149">Verify that the VM was created</span></span>
<span data-ttu-id="e13bb-150">Po dokončení byste měli vidět nově vytvořený virtuální počítač v [portál Azure](https://portal.azure.com) pod **Procházet** > **virtuální počítače**, nebo pomocí následujících příkazů prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="e13bb-150">When complete, you should see the newly created VM in the [Azure portal](https://portal.azure.com) under **Browse** > **Virtual machines**, or by using the following PowerShell commands:</span></span>

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="e13bb-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e13bb-151">Next steps</span></span>
<span data-ttu-id="e13bb-152">Ke správě nového virtuálního počítače v prostředí Azure PowerShell najdete [vytvořit a spravovat virtuální počítače Windows pomocí modulu Azure PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e13bb-152">To manage your new virtual machine with Azure PowerShell, see [Create and manage Windows VMs with the Azure PowerShell module](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

