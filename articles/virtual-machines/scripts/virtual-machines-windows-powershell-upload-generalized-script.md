---
title: "Nahrání zobecněný virtuální pevný disk do Azure ukázka skriptu prostředí PowerShell | Microsoft Docs"
description: "Ukázkový skript prostředí PowerShell nahrajte zobecněný virtuální pevný disk do Azure a vytvoření nového virtuálního počítače pomocí modelu nasazení resource manager a spravované disky."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/18/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: cd3d87bb4384971e28d3330cd5c1a3d351129036
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sample-script-to-upload-a-vhd-to-azure-and-create-a-new-vm"></a><span data-ttu-id="c28cf-103">Ukázkový skript pro nahrání virtuálního pevného disku do Azure a vytvoření nového virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="c28cf-103">Sample script to upload a VHD to Azure and create a new VM</span></span>

<span data-ttu-id="c28cf-104">Tento skript trvá soubor VHD místní z zobecněný virtuální počítač a odesílá do Azure, vytvoří bitovou kopii disku spravované a použije k vytvoření nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c28cf-104">This script takes a local .vhd file from a generalized VM and uploads it to Azure, creates a Managed Disk image and uses the to create a new VM.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="c28cf-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="c28cf-105">Sample script</span></span>

```powershell
# Provide values for the variables
$resourceGroup = 'myResourceGroup'
$location = 'EastUS'
$storageaccount = 'mystorageaccount'
$storageType = 'Standard_LRS'
$containername = 'mycontainer'
$localPath = 'C:\Users\Public\Documents\Hyper-V\VHDs\generalized.vhd'
$vmName = 'myVM'
$imageName = 'myImage'
$vhdName = 'myUploadedVhd.vhd'
$diskSizeGB = '128'
$subnetName = 'mySubnet'
$vnetName = 'myVnet'
$ipName = 'myPip'
$nicName = 'myNic'
$nsgName = 'myNsg'
$ruleName = 'myRdpRule'
$vmName = 'myVM'
$computerName = 'myComputerName'
$vmSize = 'Standard_DS1_v2'

# Get the username and password to be used for the administrators account on the VM. 
# This is used when connecting to the VM using RDP.

$cred = Get-Credential

# Upload the VHD
New-AzureRmResourceGroup -Name $resourceGroup -Location $location
New-AzureRmStorageAccount -ResourceGroupName $resourceGroup -Name $storageAccount -Location $location `
    -SkuName $storageType -Kind "Storage"
$urlOfUploadedImageVhd = ('https://' + $storageaccount + '.blob.core.windows.net/' + $containername + '/' + $vhdName)
Add-AzureRmVhd -ResourceGroupName $resourceGroup -Destination $urlOfUploadedImageVhd `
    -LocalFilePath $localPath

# Note: Uploading the VHD may take awhile!

# Create a managed image from the uploaded VHD 
$imageConfig = New-AzureRmImageConfig -Location $location
$imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized `
    -BlobUri $urlOfUploadedImageVhd
$image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $resourceGroup -Image $imageConfig
 
# Create the networking resources
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroup -Location $location `
    -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
$pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $resourceGroup -Location $location `
    -AllocationMethod Dynamic
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $resourceGroup -Location $location `
    -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description 'Allow RDP' -Access Allow `
    -Protocol Tcp -Direction Inbound -Priority 110 -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Name $vnetName

# Start building the VM configuration
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize

# Set the VM image as source image for the new VM
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id

# Finish the VM configuration and add the NIC.
$vm = Set-AzureRmVMOSDisk -VM $vm  -DiskSizeInGB $diskSizeGB -CreateOption FromImage -Caching ReadWrite
$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id

# Create the VM
New-AzureRmVM -VM $vm -ResourceGroupName $resourceGroup -Location $location

# Verify that the VM was created
$vmList = Get-AzureRmVM -ResourceGroupName $resourceGroup
$vmList.Name


```


<!-- 
[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-iis/create-windows-vm-iis.ps1 "Create VM IIS")] -->

## <a name="clean-up-deployment"></a><span data-ttu-id="c28cf-106">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="c28cf-106">Clean up deployment</span></span> 

<span data-ttu-id="c28cf-107">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="c28cf-107">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="c28cf-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="c28cf-108">Script explanation</span></span>

<span data-ttu-id="c28cf-109">Tento skript používá následující příkazy k vytvoření nasazení.</span><span class="sxs-lookup"><span data-stu-id="c28cf-109">This script uses the following commands to create the deployment.</span></span> <span data-ttu-id="c28cf-110">Každou položku v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="c28cf-110">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="c28cf-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="c28cf-111">Command</span></span>                                                                                                             | <span data-ttu-id="c28cf-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="c28cf-112">Notes</span></span>                                                                                                                                                                                |
|---------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [<span data-ttu-id="c28cf-113">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="c28cf-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup)                           | <span data-ttu-id="c28cf-114">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="c28cf-114">Creates a resource group in which all resources are stored.</span></span>                                                                                                                          |
| [<span data-ttu-id="c28cf-115">Nové AzureRmStorageAccount</span><span class="sxs-lookup"><span data-stu-id="c28cf-115">New-AzureRmStorageAccount</span></span>](/powershell/module/azurerm.resources/new-azurermstorageaccount)                         | <span data-ttu-id="c28cf-116">Vytvoří účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="c28cf-116">Creates a storage account.</span></span>                                                                                                                                                           |
| [<span data-ttu-id="c28cf-117">Přidat AzureRmVhd</span><span class="sxs-lookup"><span data-stu-id="c28cf-117">Add-AzureRmVhd</span></span>](/powershell/module/azurerm.resources/add-azurermvhd)                                               | <span data-ttu-id="c28cf-118">Ukládání virtuální pevný disk z virtuálního počítače místní do objektu blob v účtu úložiště cloudu v Azure.</span><span class="sxs-lookup"><span data-stu-id="c28cf-118">Uploads a virtual hard disk from an on-premises virtual machine to a blob in a cloud storage account in Azure.</span></span>                                                                       |
| [<span data-ttu-id="c28cf-119">Nové AzureRmImageConfig</span><span class="sxs-lookup"><span data-stu-id="c28cf-119">New-AzureRmImageConfig</span></span>](/powershell/module/azurerm.resources/new-azurermimageconfig)                               | <span data-ttu-id="c28cf-120">Vytvoří objekt konfigurovat bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="c28cf-120">Creates a configurable image object.</span></span>                                                                                                                                                 |
| [<span data-ttu-id="c28cf-121">Set-AzureRmImageOsDisk</span><span class="sxs-lookup"><span data-stu-id="c28cf-121">Set-AzureRmImageOsDisk</span></span>](/powershell/module/azurerm.resources/set-azurermimageosdisk)                               | <span data-ttu-id="c28cf-122">Nastaví vlastnosti disku operačního systému na objekt bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="c28cf-122">Sets the operating system disk properties on an image object.</span></span>                                                                                                                        |
| [<span data-ttu-id="c28cf-123">Nové AzureRmImage</span><span class="sxs-lookup"><span data-stu-id="c28cf-123">New-AzureRmImage</span></span>](/powershell/module/azurerm.resources/new-azurermimage)                                           | <span data-ttu-id="c28cf-124">Vytvoří novou bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="c28cf-124">Creates a new image.</span></span>                                                                                                                                                                 |
| [<span data-ttu-id="c28cf-125">Nové AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="c28cf-125">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.resources/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="c28cf-126">Vytvoří konfiguraci podsítě.</span><span class="sxs-lookup"><span data-stu-id="c28cf-126">Creates a subnet configuration.</span></span> <span data-ttu-id="c28cf-127">Tato konfigurace se používá s procesem vytvoření virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="c28cf-127">This configuration is used with the virtual network creation process.</span></span>                                                                                |
| [<span data-ttu-id="c28cf-128">Nový AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="c28cf-128">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.resources/new-azurermvirtualnetwork)                         | <span data-ttu-id="c28cf-129">Vytvoří virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="c28cf-129">Creates a virtual network.</span></span>                                                                                                                                                           |
| [<span data-ttu-id="c28cf-130">Nové AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="c28cf-130">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.resources/new-azurermpublicipaddress)                       | <span data-ttu-id="c28cf-131">Vytvoří veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="c28cf-131">Creates a public IP address.</span></span>                                                                                                                                                         |
| [<span data-ttu-id="c28cf-132">Nové AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="c28cf-132">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.resources/new-azurermnetworkinterface)                     | <span data-ttu-id="c28cf-133">Vytvoří rozhraní sítě.</span><span class="sxs-lookup"><span data-stu-id="c28cf-133">Creates a network interface.</span></span>                                                                                                                                                         |
| [<span data-ttu-id="c28cf-134">Nové AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="c28cf-134">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.resources/new-azurermnetworksecurityruleconfig)   | <span data-ttu-id="c28cf-135">Vytvoří pravidlo konfiguraci skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="c28cf-135">Creates a network security group rule configuration.</span></span> <span data-ttu-id="c28cf-136">Tato konfigurace slouží k vytvoření pravidla NSG, když je vytvořen NSG.</span><span class="sxs-lookup"><span data-stu-id="c28cf-136">This configuration is used to create an NSG rule when the NSG is created.</span></span>                                                       |
| [<span data-ttu-id="c28cf-137">Nové AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="c28cf-137">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.resources/new-azurermnetworksecuritygroup)             | <span data-ttu-id="c28cf-138">Vytvoří skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="c28cf-138">Creates a network security group.</span></span>                                                                                                                                                    |
| [<span data-ttu-id="c28cf-139">Get-AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="c28cf-139">Get-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.resources/get-azurermvirtualnetwork)                         | <span data-ttu-id="c28cf-140">Získá virtuální síť ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="c28cf-140">Gets a virtual network in a resource group.</span></span>                                                                                                                                          |
| [<span data-ttu-id="c28cf-141">Nové AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="c28cf-141">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.resources/new-azurermvmconfig)                                     | <span data-ttu-id="c28cf-142">Vytvoří konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c28cf-142">Creates a VM configuration.</span></span> <span data-ttu-id="c28cf-143">Tato konfigurace zahrnuje informace, jako je název virtuálního počítače, operační systém a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="c28cf-143">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="c28cf-144">Konfigurace se používá při vytváření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c28cf-144">The configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="c28cf-145">Set-AzureRmVMSourceImage</span><span class="sxs-lookup"><span data-stu-id="c28cf-145">Set-AzureRmVMSourceImage</span></span>](/powershell/module/azurerm.resources/set-azurermvmsourceimage)                           | <span data-ttu-id="c28cf-146">Určuje obrázek, který pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="c28cf-146">Specifies an image for a virtual machine.</span></span>                                                                                                                                            |
| [<span data-ttu-id="c28cf-147">Set-AzureRmVMOSDisk</span><span class="sxs-lookup"><span data-stu-id="c28cf-147">Set-AzureRmVMOSDisk</span></span>](/powershell/module/azurerm.resources/set-azurermvmosdisk)                                     | <span data-ttu-id="c28cf-148">Nastaví vlastnosti disku operačního systému na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="c28cf-148">Sets the operating system disk properties on a virtual machine.</span></span>                                                                                                                      |
| [<span data-ttu-id="c28cf-149">Set-AzureRmVMOperatingSystem</span><span class="sxs-lookup"><span data-stu-id="c28cf-149">Set-AzureRmVMOperatingSystem</span></span>](/powershell/module/azurerm.resources/set-azurermvmoperatingsystem)                   | <span data-ttu-id="c28cf-150">Nastaví vlastnosti disku operačního systému na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="c28cf-150">Sets the operating system disk properties on a virtual machine.</span></span>                                                                                                                      |
| [<span data-ttu-id="c28cf-151">Přidat AzureRmVMNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="c28cf-151">Add-AzureRmVMNetworkInterface</span></span>](/powershell/module/azurerm.resources/add-azurermvmnetworkinterface)                 | <span data-ttu-id="c28cf-152">Přidá rozhraní sítě k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="c28cf-152">Adds a network interface to a virtual machine.</span></span>                                                                                                                                       |
| [<span data-ttu-id="c28cf-153">Nový AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="c28cf-153">New-AzureRmVM</span></span>](/powershell/module/azurerm.resources/new-azurermvm)                                                 | <span data-ttu-id="c28cf-154">Vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c28cf-154">Create a virtual machine.</span></span>                                                                                                                                                            |
| [<span data-ttu-id="c28cf-155">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="c28cf-155">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup)                     | <span data-ttu-id="c28cf-156">Odebere skupinu prostředků a všechny prostředky obsažené v rámci.</span><span class="sxs-lookup"><span data-stu-id="c28cf-156">Removes a resource group and all resources contained within.</span></span>                                                                                                                         |

## <a name="next-steps"></a><span data-ttu-id="c28cf-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c28cf-157">Next steps</span></span>

<span data-ttu-id="c28cf-158">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c28cf-158">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="c28cf-159">Ukázky skriptu PowerShell další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c28cf-159">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
