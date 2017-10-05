---
title: "Vytvářet a spravovat virtuální počítače Windows v Azure, použít několik síťových adaptérů | Microsoft Docs"
description: "Naučte se vytvářet a spravovat virtuální počítač Windows, který má několik síťových adaptérů připojený pomocí šablon Azure PowerShell nebo správce prostředků."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 9bff5b6d-79ac-476b-a68f-6f8754768413
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/05/2017
ms.author: iainfou
ms.openlocfilehash: 3bd99a67dae41de3533d7f6e244eb7ee3ecc4049
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-a-windows-virtual-machine-that-has-multiple-nics"></a><span data-ttu-id="a2482-103">Vytvoření a Správa virtuálního počítače s Windows, který má několik síťových adaptérů</span><span class="sxs-lookup"><span data-stu-id="a2482-103">Create and manage a Windows virtual machine that has multiple NICs</span></span>
<span data-ttu-id="a2482-104">Virtuální počítače (VM) v Azure může mít několik virtuálních síťových karet (NIC) připojené k nim.</span><span class="sxs-lookup"><span data-stu-id="a2482-104">Virtual machines (VMs) in Azure can have multiple virtual network interface cards (NICs) attached to them.</span></span> <span data-ttu-id="a2482-105">Obvyklým scénářem je mít různé podsítě pro připojení front-end a back-end nebo síť vyhrazený pro řešení monitorování nebo zálohování.</span><span class="sxs-lookup"><span data-stu-id="a2482-105">A common scenario is to have different subnets for front-end and back-end connectivity, or a network dedicated to a monitoring or backup solution.</span></span> <span data-ttu-id="a2482-106">Tento článek popisuje, jak vytvořit virtuální počítač, který má několik síťových adaptérů, které jsou k němu připojen.</span><span class="sxs-lookup"><span data-stu-id="a2482-106">This article details how to create a VM that has multiple NICs attached to it.</span></span> <span data-ttu-id="a2482-107">Můžete také zjistěte, jak přidat nebo odebrat síťové adaptéry ze stávajícího virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a2482-107">You also learn how to add or remove NICs from an existing VM.</span></span> <span data-ttu-id="a2482-108">Různé [velikosti virtuálních počítačů](sizes.md) podporu různých počet síťových adaptérů, takže odpovídajícím způsobem upravit velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a2482-108">Different [VM sizes](sizes.md) support a varying number of NICs, so size your VM accordingly.</span></span>

<span data-ttu-id="a2482-109">Podrobné informace, včetně toho, jak vytvořit několik síťových adaptérů v rámci své vlastní skripty prostředí PowerShell najdete v části [nasazení virtuálních počítačů více Síťových](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="a2482-109">For detailed information, including how to create multiple NICs within your own PowerShell scripts, see [deploying multiple-NIC VMs](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a2482-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a2482-110">Prerequisites</span></span>
<span data-ttu-id="a2482-111">Ujistěte se, že máte [nejnovější verzi prostředí Azure PowerShell nainstalovaný a nakonfigurovaný](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a2482-111">Make sure that you have the [latest Azure PowerShell version installed and configured](/powershell/azure/overview).</span></span>

<span data-ttu-id="a2482-112">V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a2482-112">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="a2482-113">Zahrnout názvy parametrů příklad *myResourceGroup*, *myVnet*, a *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="a2482-113">Example parameter names include *myResourceGroup*, *myVnet*, and *myVM*.</span></span>


## <a name="create-a-vm-with-multiple-nics"></a><span data-ttu-id="a2482-114">Vytvoření virtuálního počítače s několika síťovými kartami</span><span class="sxs-lookup"><span data-stu-id="a2482-114">Create a VM with multiple NICs</span></span>
<span data-ttu-id="a2482-115">Nejprve vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="a2482-115">First, create a resource group.</span></span> <span data-ttu-id="a2482-116">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v *EastUs* umístění:</span><span class="sxs-lookup"><span data-stu-id="a2482-116">The following example creates a resource group named *myResourceGroup* in the *EastUs* location:</span></span>

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

### <a name="create-virtual-network-and-subnets"></a><span data-ttu-id="a2482-117">Vytvoření virtuální sítě a podsítě</span><span class="sxs-lookup"><span data-stu-id="a2482-117">Create virtual network and subnets</span></span>
<span data-ttu-id="a2482-118">Obvyklým scénářem je pro virtuální síť má dva nebo více podsítí.</span><span class="sxs-lookup"><span data-stu-id="a2482-118">A common scenario is for a virtual network to have two or more subnets.</span></span> <span data-ttu-id="a2482-119">Jedna podsíť může být pro front-endu provoz, druhý pro přenosy back-end.</span><span class="sxs-lookup"><span data-stu-id="a2482-119">One subnet may be for front-end traffic, the other for back-end traffic.</span></span> <span data-ttu-id="a2482-120">Pro připojení k obě podsítě, potom použijte několik síťových adaptérů na vašem virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="a2482-120">To connect to both subnets, you then use multiple NICs on your VM.</span></span>

1. <span data-ttu-id="a2482-121">Definovat dvě podsítě virtuální sítě s [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig).</span><span class="sxs-lookup"><span data-stu-id="a2482-121">Define two virtual network subnets with [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig).</span></span> <span data-ttu-id="a2482-122">V následujícím příkladu definuje podsítě pro *mySubnetFrontEnd* a *mySubnetBackEnd*:</span><span class="sxs-lookup"><span data-stu-id="a2482-122">The following example defines the subnets for *mySubnetFrontEnd* and *mySubnetBackEnd*:</span></span>

    ```powershell
    $mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
        -AddressPrefix "192.168.1.0/24"
    $mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
        -AddressPrefix "192.168.2.0/24"
    ```

2. <span data-ttu-id="a2482-123">Vytvoření virtuální sítě a podsítě s [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span><span class="sxs-lookup"><span data-stu-id="a2482-123">Create your virtual network and subnets with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span></span> <span data-ttu-id="a2482-124">Následující příklad vytvoří virtuální síť s názvem *myVnet*:</span><span class="sxs-lookup"><span data-stu-id="a2482-124">The following example creates a virtual network named *myVnet*:</span></span>

    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
        -Location "EastUs" `
        -Name "myVnet" `
        -AddressPrefix "192.168.0.0/16" `
        -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
    ```


### <a name="create-multiple-nics"></a><span data-ttu-id="a2482-125">Vytvořit několik síťových adaptérů</span><span class="sxs-lookup"><span data-stu-id="a2482-125">Create multiple NICs</span></span>
<span data-ttu-id="a2482-126">Vytvořte dva síťové adaptéry se [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span><span class="sxs-lookup"><span data-stu-id="a2482-126">Create two NICs with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span></span> <span data-ttu-id="a2482-127">Jeden síťový adaptér a front-end podsítě a jeden síťový adaptér připojte k podsíti back-end.</span><span class="sxs-lookup"><span data-stu-id="a2482-127">Attach one NIC to the front-end subnet and one NIC to the back-end subnet.</span></span> <span data-ttu-id="a2482-128">Následující příklad vytvoří síťové adaptéry s názvem *myNic1* a *myNic2*:</span><span class="sxs-lookup"><span data-stu-id="a2482-128">The following example creates NICs named *myNic1* and *myNic2*:</span></span>

```powershell
$frontEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetFrontEnd'}
$myNic1 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic1" `
    -Location "EastUs" `
    -SubnetId $frontEnd.Id

$backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}
$myNic2 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic2" `
    -Location "EastUs" `
    -SubnetId $backEnd.Id
```

<span data-ttu-id="a2482-129">Obvykle můžete také vytvořit [skupinu zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md) nebo [nástroj pro vyrovnávání zatížení](../../load-balancer/load-balancer-overview.md) ke správě a distribuci přenosů mezi virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="a2482-129">Typically you also create a [network security group](../../virtual-network/virtual-networks-nsg.md) or [load balancer](../../load-balancer/load-balancer-overview.md) to help manage and distribute traffic across your VMs.</span></span> <span data-ttu-id="a2482-130">[Podrobnější virtuální počítač více Síťových](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md) článek vás provede vytvoření skupiny zabezpečení sítě a přiřazení síťových karet.</span><span class="sxs-lookup"><span data-stu-id="a2482-130">The [more detailed multiple-NIC VM](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md) article guides you through creating a network security group and assigning NICs.</span></span>

### <a name="create-the-virtual-machine"></a><span data-ttu-id="a2482-131">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="a2482-131">Create the virtual machine</span></span>
<span data-ttu-id="a2482-132">Nyní spusťte Sestavit vaše konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a2482-132">Now start to build your VM configuration.</span></span> <span data-ttu-id="a2482-133">Limit velikost každého virtuálního počítače je celkový počet síťových adaptérů, které můžete přidat k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="a2482-133">Each VM size has a limit for the total number of NICs that you can add to a VM.</span></span> <span data-ttu-id="a2482-134">Další informace najdete v tématu [velikosti virtuálních počítačů Windows](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="a2482-134">For more information, see [Windows VM sizes](sizes.md).</span></span>

1. <span data-ttu-id="a2482-135">Nastavte přihlašovací údaje virtuálních počítačů `$cred` proměnná následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a2482-135">Set your VM credentials to the `$cred` variable as follows:</span></span>

    ```powershell
    $cred = Get-Credential
    ```

2. <span data-ttu-id="a2482-136">Zadejte virtuální počítač s [nové AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig).</span><span class="sxs-lookup"><span data-stu-id="a2482-136">Define your VM with [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig).</span></span> <span data-ttu-id="a2482-137">V následujícím příkladu definuje virtuální počítač s názvem *Můjvp* a používá velikost virtuálního počítače, který podporuje více než dva síťové adaptéry (*Standard_DS3_v2*):</span><span class="sxs-lookup"><span data-stu-id="a2482-137">The following example defines a VM named *myVM* and uses a VM size that supports more than two NICs (*Standard_DS3_v2*):</span></span>

    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS3_v2"
    ```

3. <span data-ttu-id="a2482-138">Vytvoření zbytek konfiguraci virtuálních počítačů s [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) a [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage).</span><span class="sxs-lookup"><span data-stu-id="a2482-138">Create the rest of your VM configuration with [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) and [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage).</span></span> <span data-ttu-id="a2482-139">Následující příklad vytvoří virtuální počítač s Windows Server 2016:</span><span class="sxs-lookup"><span data-stu-id="a2482-139">The following example creates a Windows Server 2016 VM:</span></span>

    ```powershell
    $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig `
        -Windows `
        -ComputerName "myVM" `
        -Credential $cred `
        -ProvisionVMAgent `
        -EnableAutoUpdate
    $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig `
        -PublisherName "MicrosoftWindowsServer" `
        -Offer "WindowsServer" `
        -Skus "2016-Datacenter" `
        -Version "latest"
   ```

4. <span data-ttu-id="a2482-140">Připojit dvěma síťovými adaptéry, které jste předtím vytvořili pomocí [přidat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="a2482-140">Attach the two NICs that you previously created with [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span></span>

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
    ```

5. <span data-ttu-id="a2482-141">Nakonec vytvořte virtuální počítač s [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="a2482-141">Finally, create your VM with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm):</span></span>

    ```powershell
    New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "EastUs"
    ```

## <a name="add-a-nic-to-an-existing-vm"></a><span data-ttu-id="a2482-142">Přidat síťovou kartu k existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="a2482-142">Add a NIC to an existing VM</span></span>
<span data-ttu-id="a2482-143">Pokud chcete přidat do existující virtuální počítač virtuální síťovou kartu, můžete zrušit přidělení virtuálního počítače, přidejte virtuální síťovou kartu a pak spusťte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a2482-143">To add a virtual NIC to an existing VM, you deallocate the VM, add the virtual NIC, then start the VM.</span></span>

1. <span data-ttu-id="a2482-144">Zrušit přidělení virtuálního počítače s [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="a2482-144">Deallocate the VM with [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span></span> <span data-ttu-id="a2482-145">Následující příklad zruší přidělení virtuálního počítače s názvem *Můjvp* v *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="a2482-145">The following example deallocates the VM named *myVM* in *myResourceGroup*:</span></span>

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. <span data-ttu-id="a2482-146">Získat existující konfiguraci virtuálního počítače s [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="a2482-146">Get the existing configuration of the VM with [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm).</span></span> <span data-ttu-id="a2482-147">Následující příklad načte informace pro virtuální počítač s názvem *Můjvp* v *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="a2482-147">The following example gets information for the VM named *myVM* in *myResourceGroup*:</span></span>

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. <span data-ttu-id="a2482-148">Následující příklad vytvoří virtuální síťový adaptér s [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) s názvem *myNic3* připojená k *mySubnetBackEnd*.</span><span class="sxs-lookup"><span data-stu-id="a2482-148">The following example creates a virtual NIC with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) named *myNic3* that is attached to *mySubnetBackEnd*.</span></span> <span data-ttu-id="a2482-149">Poté virtuální síťový adaptér je připojen k virtuální počítač s názvem *Můjvp* v *myResourceGroup* s [přidat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="a2482-149">The virtual NIC is then attached to the VM named *myVM* in *myResourceGroup* with [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span></span>

    ```powershell
    # Get info for the back end subnet
    $myVnet = Get-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName "myResourceGroup"
    $backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}

    # Create a virtual NIC
    $myNic3 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
        -Name "myNic3" `
        -Location "EastUs" `
        -SubnetId $backEnd.Id

    # Get the ID of the new virtual NIC and add to VM
    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "MyNic3").Id
    Add-AzureRmVMNetworkInterface -VM $vm -Id $nicId | Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```

    ### <a name="primary-virtual-nics"></a><span data-ttu-id="a2482-150">Primární virtuální síťové karty</span><span class="sxs-lookup"><span data-stu-id="a2482-150">Primary virtual NICs</span></span>
    <span data-ttu-id="a2482-151">Mezi síťovými adaptéry na virtuálním počítači více síťovými Kartami, je potřeba primární.</span><span class="sxs-lookup"><span data-stu-id="a2482-151">One of the NICs on a multi-NIC VM needs to be primary.</span></span> <span data-ttu-id="a2482-152">Pokud jeden z existující virtuální síťové adaptéry na virtuálním počítači je již nastavena jako primární, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="a2482-152">If one of the existing virtual NICs on the VM is already set as primary, you can skip this step.</span></span> <span data-ttu-id="a2482-153">Následující příklad předpokládá, že dva virtuální síťové karty jsou teď na virtuální počítač a chcete přidat první síťový adaptér (`[0]`) jako primární:</span><span class="sxs-lookup"><span data-stu-id="a2482-153">The following example assumes that two virtual NICs are now present on a VM and you wish to add the first NIC (`[0]`) as the primary:</span></span>
        
    ```powershell
    # List existing NICs on the VM and find which one is primary
    $vm.NetworkProfile.NetworkInterfaces
    
    # Set NIC 0 to be primary
    $vm.NetworkProfile.NetworkInterfaces[0].Primary = $true
    $vm.NetworkProfile.NetworkInterfaces[1].Primary = $false
    
    # Update the VM state in Azure
    Update-AzureRmVM -VM $vm -ResourceGroupName "myResourceGroup"
    ```

4. <span data-ttu-id="a2482-154">Spusťte virtuální počítač s [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="a2482-154">Start the VM with [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):</span></span>

    ```powershell
    Start-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

## <a name="remove-a-nic-from-an-existing-vm"></a><span data-ttu-id="a2482-155">Odeberte síťový adaptér ze stávajícího virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="a2482-155">Remove a NIC from an existing VM</span></span>
<span data-ttu-id="a2482-156">Odebrat virtuální síťový adaptér ze stávajícího virtuálního počítače, můžete zrušit přidělení virtuálního počítače, odeberte virtuálního síťového adaptéru, a pak spusťte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a2482-156">To remove a virtual NIC from an existing VM, you deallocate the VM, remove the virtual NIC, then start the VM.</span></span>

1. <span data-ttu-id="a2482-157">Zrušit přidělení virtuálního počítače s [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="a2482-157">Deallocate the VM with [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span></span> <span data-ttu-id="a2482-158">Následující příklad zruší přidělení virtuálního počítače s názvem *Můjvp* v *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="a2482-158">The following example deallocates the VM named *myVM* in *myResourceGroup*:</span></span>

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. <span data-ttu-id="a2482-159">Získat existující konfiguraci virtuálního počítače s [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="a2482-159">Get the existing configuration of the VM with [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm).</span></span> <span data-ttu-id="a2482-160">Následující příklad načte informace pro virtuální počítač s názvem *Můjvp* v *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="a2482-160">The following example gets information for the VM named *myVM* in *myResourceGroup*:</span></span>

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. <span data-ttu-id="a2482-161">Získat informace o odebrání síťový adaptér s [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface).</span><span class="sxs-lookup"><span data-stu-id="a2482-161">Get information about the NIC remove with [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface).</span></span> <span data-ttu-id="a2482-162">Následující příklad získá informace *myNic3*:</span><span class="sxs-lookup"><span data-stu-id="a2482-162">The following example gets information about *myNic3*:</span></span>

    ```powershell
    # List existing NICs on the VM if you need to determine NIC name
    $vm.NetworkProfile.NetworkInterfaces

    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "myNic3").Id   
    ```

4. <span data-ttu-id="a2482-163">Odeberte na síťový adaptér s [odebrat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface) a aktualizujte virtuální počítač s [aktualizace-AzureRmVm](/powershell/module/azurerm.compute/update-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="a2482-163">Remove the NIC with [Remove-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface) and then update the VM with [Update-AzureRmVm](/powershell/module/azurerm.compute/update-azurermvm).</span></span> <span data-ttu-id="a2482-164">Následující příklad odebere *myNic3* získány `$nicId` v předchozím kroku:</span><span class="sxs-lookup"><span data-stu-id="a2482-164">The following example removes *myNic3* as obtained by `$nicId` in the preceding step:</span></span>

    ```powershell
    Remove-AzureRmVMNetworkInterface -VM $vm -NetworkInterfaceIDs $nicId | `
        Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```   

5. <span data-ttu-id="a2482-165">Spusťte virtuální počítač s [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="a2482-165">Start the VM with [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):</span></span>

    ```powershell
    Start-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```   

## <a name="create-multiple-nics-with-templates"></a><span data-ttu-id="a2482-166">Vytvořit několik síťových adaptérů se šablonami</span><span class="sxs-lookup"><span data-stu-id="a2482-166">Create multiple NICs with templates</span></span>
<span data-ttu-id="a2482-167">Šablony Azure Resource Manageru poskytují způsob, jak vytvořit více instancí prostředku během nasazení, jako je například vytváření několik síťových adaptérů.</span><span class="sxs-lookup"><span data-stu-id="a2482-167">Azure Resource Manager templates provide a way to create multiple instances of a resource during deployment, such as creating multiple NICs.</span></span> <span data-ttu-id="a2482-168">Deklarativní soubory JSON šablony Resource Manageru použít k definování prostředí.</span><span class="sxs-lookup"><span data-stu-id="a2482-168">Resource Manager templates use declarative JSON files to define your environment.</span></span> <span data-ttu-id="a2482-169">Další informace najdete v tématu [přehled nástroje Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a2482-169">For more information, see [overview of Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="a2482-170">Můžete použít *kopie* k určení počtu instancí vytvořit:</span><span class="sxs-lookup"><span data-stu-id="a2482-170">You can use *copy* to specify the number of instances to create:</span></span>

```json
"copy": {
    "name": "multiplenics",
    "count": "[parameters('count')]"
}
```

<span data-ttu-id="a2482-171">Další informace najdete v tématu [vytvoření více instancí pomocí *kopie*](../../resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="a2482-171">For more information, see [creating multiple instances by using *copy*](../../resource-group-create-multiple.md).</span></span> 

<span data-ttu-id="a2482-172">Můžete také použít `copyIndex()` číslo připojit k názvu zdroje.</span><span class="sxs-lookup"><span data-stu-id="a2482-172">You can also use `copyIndex()` to append a number to a resource name.</span></span> <span data-ttu-id="a2482-173">Potom můžete vytvořit *myNic1*, *MyNic2* a tak dále.</span><span class="sxs-lookup"><span data-stu-id="a2482-173">You can then create *myNic1*, *MyNic2* and so on.</span></span> <span data-ttu-id="a2482-174">Následující kód ukazuje příklad připojením index hodnoty:</span><span class="sxs-lookup"><span data-stu-id="a2482-174">The following code shows an example of appending the index value:</span></span>

```json
"name": "[concat('myNic', copyIndex())]", 
```

<span data-ttu-id="a2482-175">Kompletní příklad, jak si můžete přečíst [vytvoření několik síťových adaptérů pomocí šablony Resource Manageru](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="a2482-175">You can read a complete example of [creating multiple NICs by using Resource Manager templates](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a2482-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a2482-176">Next steps</span></span>
<span data-ttu-id="a2482-177">Zkontrolujte [velikosti virtuálních počítačů Windows](sizes.md) při pokusu o vytvoření virtuálního počítače, který má několik síťových adaptérů.</span><span class="sxs-lookup"><span data-stu-id="a2482-177">Review [Windows VM sizes](sizes.md) when you're trying to create a VM that has multiple NICs.</span></span> <span data-ttu-id="a2482-178">Věnujte pozornost maximální počet síťových adaptérů, které podporuje každý velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a2482-178">Pay attention to the maximum number of NICs that each VM size supports.</span></span> 


