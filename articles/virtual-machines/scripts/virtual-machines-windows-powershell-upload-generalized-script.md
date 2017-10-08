---
title: "aaaUpload zobecněný virtuální pevný disk tooAzure ukázkový skript prostředí PowerShell | Microsoft Docs"
description: "Prostředí PowerShell ukázkový skript tooupload zobecněný virtuální pevný disk tooAzure a vytvořte nový virtuální počítač pomocí modelu nasazení resource manager hello a spravované disky."
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
ms.openlocfilehash: 72b4ff09eb7a6ba1604b9d5cbac51e60eded7545
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sample-script-tooupload-a-vhd-tooazure-and-create-a-new-vm"></a><span data-ttu-id="de30a-103">Ukázkový skript tooupload tooAzure virtuální pevný disk a vytvořte nový virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="de30a-103">Sample script tooupload a VHD tooAzure and create a new VM</span></span>

<span data-ttu-id="de30a-104">Tento skript trvá soubor VHD místní z zobecněný virtuální počítač a odešle ji tooAzure, vytvoří bitovou kopii disku spravované a použije hello toocreate nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="de30a-104">This script takes a local .vhd file from a generalized VM and uploads it tooAzure, creates a Managed Disk image and uses hello toocreate a new VM.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="de30a-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="de30a-105">Sample script</span></span>

```powershell
# Provide values for hello variables
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

# Get hello username and password toobe used for hello administrators account on hello VM. 
# This is used when connecting toohello VM using RDP.

$cred = Get-Credential

# Upload hello VHD
New-AzureRmResourceGroup -Name $resourceGroup -Location $location
New-AzureRmStorageAccount -ResourceGroupName $resourceGroup -Name $storageAccount -Location $location `
    -SkuName $storageType -Kind "Storage"
$urlOfUploadedImageVhd = ('https://' + $storageaccount + '.blob.core.windows.net/' + $containername + '/' + $vhdName)
Add-AzureRmVhd -ResourceGroupName $resourceGroup -Destination $urlOfUploadedImageVhd `
    -LocalFilePath $localPath

# Note: Uploading hello VHD may take awhile!

# Create a managed image from hello uploaded VHD 
$imageConfig = New-AzureRmImageConfig -Location $location
$imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized `
    -BlobUri $urlOfUploadedImageVhd
$image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $resourceGroup -Image $imageConfig
 
# Create hello networking resources
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

# Start building hello VM configuration
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize

# Set hello VM image as source image for hello new VM
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id

# Finish hello VM configuration and add hello NIC.
$vm = Set-AzureRmVMOSDisk -VM $vm  -DiskSizeInGB $diskSizeGB -CreateOption FromImage -Caching ReadWrite
$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id

# Create hello VM
New-AzureRmVM -VM $vm -ResourceGroupName $resourceGroup -Location $location

# Verify that hello VM was created
$vmList = Get-AzureRmVM -ResourceGroupName $resourceGroup
$vmList.Name


```


<!-- 
[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-iis/create-windows-vm-iis.ps1 "Create VM IIS")] -->

## <a name="clean-up-deployment"></a><span data-ttu-id="de30a-106">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="de30a-106">Clean up deployment</span></span> 

<span data-ttu-id="de30a-107">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="de30a-107">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="de30a-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="de30a-108">Script explanation</span></span>

<span data-ttu-id="de30a-109">Tento skript používá hello následující příkazy toocreate hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="de30a-109">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="de30a-110">Každou položku v tabulce hello propojí toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="de30a-110">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="de30a-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="de30a-111">Command</span></span>                                                                                                             | <span data-ttu-id="de30a-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="de30a-112">Notes</span></span>                                                                                                                                                                                |
|---------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [<span data-ttu-id="de30a-113">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="de30a-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup)                           | <span data-ttu-id="de30a-114">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="de30a-114">Creates a resource group in which all resources are stored.</span></span>                                                                                                                          |
| [<span data-ttu-id="de30a-115">Nové AzureRmStorageAccount</span><span class="sxs-lookup"><span data-stu-id="de30a-115">New-AzureRmStorageAccount</span></span>](/powershell/module/azurerm.resources/new-azurermstorageaccount)                         | <span data-ttu-id="de30a-116">Vytvoří účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="de30a-116">Creates a storage account.</span></span>                                                                                                                                                           |
| [<span data-ttu-id="de30a-117">Přidat AzureRmVhd</span><span class="sxs-lookup"><span data-stu-id="de30a-117">Add-AzureRmVhd</span></span>](/powershell/module/azurerm.resources/add-azurermvhd)                                               | <span data-ttu-id="de30a-118">Ukládání virtuální pevný disk z objektu blob pro tooa místní virtuální počítač v cloudu účtu úložiště v Azure.</span><span class="sxs-lookup"><span data-stu-id="de30a-118">Uploads a virtual hard disk from an on-premises virtual machine tooa blob in a cloud storage account in Azure.</span></span>                                                                       |
| [<span data-ttu-id="de30a-119">Nové AzureRmImageConfig</span><span class="sxs-lookup"><span data-stu-id="de30a-119">New-AzureRmImageConfig</span></span>](/powershell/module/azurerm.resources/new-azurermimageconfig)                               | <span data-ttu-id="de30a-120">Vytvoří objekt konfigurovat bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="de30a-120">Creates a configurable image object.</span></span>                                                                                                                                                 |
| [<span data-ttu-id="de30a-121">Set-AzureRmImageOsDisk</span><span class="sxs-lookup"><span data-stu-id="de30a-121">Set-AzureRmImageOsDisk</span></span>](/powershell/module/azurerm.resources/set-azurermimageosdisk)                               | <span data-ttu-id="de30a-122">Nastaví vlastnosti disku hello operačního systému na objekt bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="de30a-122">Sets hello operating system disk properties on an image object.</span></span>                                                                                                                        |
| [<span data-ttu-id="de30a-123">Nové AzureRmImage</span><span class="sxs-lookup"><span data-stu-id="de30a-123">New-AzureRmImage</span></span>](/powershell/module/azurerm.resources/new-azurermimage)                                           | <span data-ttu-id="de30a-124">Vytvoří novou bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="de30a-124">Creates a new image.</span></span>                                                                                                                                                                 |
| [<span data-ttu-id="de30a-125">Nové AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="de30a-125">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.resources/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="de30a-126">Vytvoří konfiguraci podsítě.</span><span class="sxs-lookup"><span data-stu-id="de30a-126">Creates a subnet configuration.</span></span> <span data-ttu-id="de30a-127">Tato konfigurace se používá s procesem vytvoření virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="de30a-127">This configuration is used with hello virtual network creation process.</span></span>                                                                                |
| [<span data-ttu-id="de30a-128">Nový AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="de30a-128">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.resources/new-azurermvirtualnetwork)                         | <span data-ttu-id="de30a-129">Vytvoří virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="de30a-129">Creates a virtual network.</span></span>                                                                                                                                                           |
| [<span data-ttu-id="de30a-130">Nové AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="de30a-130">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.resources/new-azurermpublicipaddress)                       | <span data-ttu-id="de30a-131">Vytvoří veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="de30a-131">Creates a public IP address.</span></span>                                                                                                                                                         |
| [<span data-ttu-id="de30a-132">Nové AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="de30a-132">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.resources/new-azurermnetworkinterface)                     | <span data-ttu-id="de30a-133">Vytvoří rozhraní sítě.</span><span class="sxs-lookup"><span data-stu-id="de30a-133">Creates a network interface.</span></span>                                                                                                                                                         |
| [<span data-ttu-id="de30a-134">Nové AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="de30a-134">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.resources/new-azurermnetworksecurityruleconfig)   | <span data-ttu-id="de30a-135">Vytvoří pravidlo konfiguraci skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="de30a-135">Creates a network security group rule configuration.</span></span> <span data-ttu-id="de30a-136">Tato konfigurace je použité toocreate pravidlo NSG při vytvoření hello NSG.</span><span class="sxs-lookup"><span data-stu-id="de30a-136">This configuration is used toocreate an NSG rule when hello NSG is created.</span></span>                                                       |
| [<span data-ttu-id="de30a-137">Nové AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="de30a-137">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.resources/new-azurermnetworksecuritygroup)             | <span data-ttu-id="de30a-138">Vytvoří skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="de30a-138">Creates a network security group.</span></span>                                                                                                                                                    |
| [<span data-ttu-id="de30a-139">Get-AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="de30a-139">Get-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.resources/get-azurermvirtualnetwork)                         | <span data-ttu-id="de30a-140">Získá virtuální síť ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="de30a-140">Gets a virtual network in a resource group.</span></span>                                                                                                                                          |
| [<span data-ttu-id="de30a-141">Nové AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="de30a-141">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.resources/new-azurermvmconfig)                                     | <span data-ttu-id="de30a-142">Vytvoří konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="de30a-142">Creates a VM configuration.</span></span> <span data-ttu-id="de30a-143">Tato konfigurace zahrnuje informace, jako je název virtuálního počítače, operační systém a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="de30a-143">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="de30a-144">Konfigurace Hello se používá při vytváření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="de30a-144">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="de30a-145">Set-AzureRmVMSourceImage</span><span class="sxs-lookup"><span data-stu-id="de30a-145">Set-AzureRmVMSourceImage</span></span>](/powershell/module/azurerm.resources/set-azurermvmsourceimage)                           | <span data-ttu-id="de30a-146">Určuje obrázek, který pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="de30a-146">Specifies an image for a virtual machine.</span></span>                                                                                                                                            |
| [<span data-ttu-id="de30a-147">Set-AzureRmVMOSDisk</span><span class="sxs-lookup"><span data-stu-id="de30a-147">Set-AzureRmVMOSDisk</span></span>](/powershell/module/azurerm.resources/set-azurermvmosdisk)                                     | <span data-ttu-id="de30a-148">Nastaví vlastnosti disku hello operační systém na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="de30a-148">Sets hello operating system disk properties on a virtual machine.</span></span>                                                                                                                      |
| [<span data-ttu-id="de30a-149">Set-AzureRmVMOperatingSystem</span><span class="sxs-lookup"><span data-stu-id="de30a-149">Set-AzureRmVMOperatingSystem</span></span>](/powershell/module/azurerm.resources/set-azurermvmoperatingsystem)                   | <span data-ttu-id="de30a-150">Nastaví vlastnosti disku hello operační systém na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="de30a-150">Sets hello operating system disk properties on a virtual machine.</span></span>                                                                                                                      |
| [<span data-ttu-id="de30a-151">Přidat AzureRmVMNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="de30a-151">Add-AzureRmVMNetworkInterface</span></span>](/powershell/module/azurerm.resources/add-azurermvmnetworkinterface)                 | <span data-ttu-id="de30a-152">Přidá síťové rozhraní tooa virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="de30a-152">Adds a network interface tooa virtual machine.</span></span>                                                                                                                                       |
| [<span data-ttu-id="de30a-153">Nový AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="de30a-153">New-AzureRmVM</span></span>](/powershell/module/azurerm.resources/new-azurermvm)                                                 | <span data-ttu-id="de30a-154">Vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="de30a-154">Create a virtual machine.</span></span>                                                                                                                                                            |
| [<span data-ttu-id="de30a-155">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="de30a-155">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup)                     | <span data-ttu-id="de30a-156">Odebere skupinu prostředků a všechny prostředky obsažené v rámci.</span><span class="sxs-lookup"><span data-stu-id="de30a-156">Removes a resource group and all resources contained within.</span></span>                                                                                                                         |

## <a name="next-steps"></a><span data-ttu-id="de30a-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="de30a-157">Next steps</span></span>

<span data-ttu-id="de30a-158">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="de30a-158">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="de30a-159">Ukázky skriptu PowerShell další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="de30a-159">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
