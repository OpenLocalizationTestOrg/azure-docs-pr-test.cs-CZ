---
title: "aaaCreate a spravovat virtuální počítače Windows v Azure, použít několik síťových adaptérů | Microsoft Docs"
description: "Zjistěte, jak toocreate a spravovat virtuální počítač Windows, který má více síťových adaptérů připojených tooit pomocí šablony Azure PowerShell nebo správce prostředků."
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
ms.openlocfilehash: c3c7d7569aca6f047238146d84b2ffccf05d4079
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-a-windows-virtual-machine-that-has-multiple-nics"></a><span data-ttu-id="062ed-103">Vytvoření a Správa virtuálního počítače s Windows, který má několik síťových adaptérů</span><span class="sxs-lookup"><span data-stu-id="062ed-103">Create and manage a Windows virtual machine that has multiple NICs</span></span>
<span data-ttu-id="062ed-104">Virtuální počítače (VM) v Azure může mít několik toothem karty (NIC) připojené rozhraní virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="062ed-104">Virtual machines (VMs) in Azure can have multiple virtual network interface cards (NICs) attached toothem.</span></span> <span data-ttu-id="062ed-105">Obvyklým scénářem je toohave různé podsítě pro připojení front-end a back-end nebo síť vyhrazený tooa monitorování nebo řešení zálohování.</span><span class="sxs-lookup"><span data-stu-id="062ed-105">A common scenario is toohave different subnets for front-end and back-end connectivity, or a network dedicated tooa monitoring or backup solution.</span></span> <span data-ttu-id="062ed-106">Tento článek podrobně popisuje, jak toocreate virtuální počítač, který má několik síťových adaptérů připojen tooit.</span><span class="sxs-lookup"><span data-stu-id="062ed-106">This article details how toocreate a VM that has multiple NICs attached tooit.</span></span> <span data-ttu-id="062ed-107">Také zjistíte, jak tooadd nebo odeberte síťové adaptéry ze stávajícího virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="062ed-107">You also learn how tooadd or remove NICs from an existing VM.</span></span> <span data-ttu-id="062ed-108">Různé [velikosti virtuálních počítačů](sizes.md) podporu různých počet síťových adaptérů, takže odpovídajícím způsobem upravit velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="062ed-108">Different [VM sizes](sizes.md) support a varying number of NICs, so size your VM accordingly.</span></span>

<span data-ttu-id="062ed-109">Podrobné informace, včetně jak toocreate několik síťových adaptérů v rámci své vlastní skripty prostředí PowerShell najdete v části [nasazení virtuálních počítačů více Síťových](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="062ed-109">For detailed information, including how toocreate multiple NICs within your own PowerShell scripts, see [deploying multiple-NIC VMs](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="062ed-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="062ed-110">Prerequisites</span></span>
<span data-ttu-id="062ed-111">Ujistěte se, že máte hello [nejnovější verzi prostředí Azure PowerShell nainstalovaný a nakonfigurovaný](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="062ed-111">Make sure that you have hello [latest Azure PowerShell version installed and configured](/powershell/azure/overview).</span></span>

<span data-ttu-id="062ed-112">Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="062ed-112">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="062ed-113">Zahrnout názvy parametrů příklad *myResourceGroup*, *myVnet*, a *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="062ed-113">Example parameter names include *myResourceGroup*, *myVnet*, and *myVM*.</span></span>


## <a name="create-a-vm-with-multiple-nics"></a><span data-ttu-id="062ed-114">Vytvoření virtuálního počítače s několika síťovými kartami</span><span class="sxs-lookup"><span data-stu-id="062ed-114">Create a VM with multiple NICs</span></span>
<span data-ttu-id="062ed-115">Nejprve vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="062ed-115">First, create a resource group.</span></span> <span data-ttu-id="062ed-116">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *EastUs* umístění:</span><span class="sxs-lookup"><span data-stu-id="062ed-116">hello following example creates a resource group named *myResourceGroup* in hello *EastUs* location:</span></span>

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

### <a name="create-virtual-network-and-subnets"></a><span data-ttu-id="062ed-117">Vytvoření virtuální sítě a podsítě</span><span class="sxs-lookup"><span data-stu-id="062ed-117">Create virtual network and subnets</span></span>
<span data-ttu-id="062ed-118">Obvyklým scénářem je pro virtuální síť toohave dvě nebo více podsítí.</span><span class="sxs-lookup"><span data-stu-id="062ed-118">A common scenario is for a virtual network toohave two or more subnets.</span></span> <span data-ttu-id="062ed-119">Jedna podsíť může být pro front-endu provoz, hello jiné pro provoz back-end.</span><span class="sxs-lookup"><span data-stu-id="062ed-119">One subnet may be for front-end traffic, hello other for back-end traffic.</span></span> <span data-ttu-id="062ed-120">tooconnect tooboth podsítě, můžete pak používat několik síťových adaptérů na vašem virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="062ed-120">tooconnect tooboth subnets, you then use multiple NICs on your VM.</span></span>

1. <span data-ttu-id="062ed-121">Definovat dvě podsítě virtuální sítě s [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig).</span><span class="sxs-lookup"><span data-stu-id="062ed-121">Define two virtual network subnets with [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig).</span></span> <span data-ttu-id="062ed-122">Hello následující příklad definuje hello podsítě pro *mySubnetFrontEnd* a *mySubnetBackEnd*:</span><span class="sxs-lookup"><span data-stu-id="062ed-122">hello following example defines hello subnets for *mySubnetFrontEnd* and *mySubnetBackEnd*:</span></span>

    ```powershell
    $mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
        -AddressPrefix "192.168.1.0/24"
    $mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
        -AddressPrefix "192.168.2.0/24"
    ```

2. <span data-ttu-id="062ed-123">Vytvoření virtuální sítě a podsítě s [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span><span class="sxs-lookup"><span data-stu-id="062ed-123">Create your virtual network and subnets with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span></span> <span data-ttu-id="062ed-124">Hello následující příklad vytvoří virtuální síť s názvem *myVnet*:</span><span class="sxs-lookup"><span data-stu-id="062ed-124">hello following example creates a virtual network named *myVnet*:</span></span>

    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
        -Location "EastUs" `
        -Name "myVnet" `
        -AddressPrefix "192.168.0.0/16" `
        -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
    ```


### <a name="create-multiple-nics"></a><span data-ttu-id="062ed-125">Vytvořit několik síťových adaptérů</span><span class="sxs-lookup"><span data-stu-id="062ed-125">Create multiple NICs</span></span>
<span data-ttu-id="062ed-126">Vytvořte dva síťové adaptéry se [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span><span class="sxs-lookup"><span data-stu-id="062ed-126">Create two NICs with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span></span> <span data-ttu-id="062ed-127">Připojte jednu podsíť front-end toohello síťový adaptér a jeden síťový adaptér toohello back-end podsíť.</span><span class="sxs-lookup"><span data-stu-id="062ed-127">Attach one NIC toohello front-end subnet and one NIC toohello back-end subnet.</span></span> <span data-ttu-id="062ed-128">Hello následující příklad vytvoří síťové adaptéry s názvem *myNic1* a *myNic2*:</span><span class="sxs-lookup"><span data-stu-id="062ed-128">hello following example creates NICs named *myNic1* and *myNic2*:</span></span>

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

<span data-ttu-id="062ed-129">Obvykle můžete také vytvořit [skupinu zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md) nebo [nástroj pro vyrovnávání zatížení](../../load-balancer/load-balancer-overview.md) toohelp spravovat a distribuovat provoz do virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="062ed-129">Typically you also create a [network security group](../../virtual-network/virtual-networks-nsg.md) or [load balancer](../../load-balancer/load-balancer-overview.md) toohelp manage and distribute traffic across your VMs.</span></span> <span data-ttu-id="062ed-130">Hello [podrobnější virtuální počítač více Síťových](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md) článek vás provede vytvoření skupiny zabezpečení sítě a přiřazení síťových karet.</span><span class="sxs-lookup"><span data-stu-id="062ed-130">hello [more detailed multiple-NIC VM](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md) article guides you through creating a network security group and assigning NICs.</span></span>

### <a name="create-hello-virtual-machine"></a><span data-ttu-id="062ed-131">Vytvoření virtuálního počítače hello</span><span class="sxs-lookup"><span data-stu-id="062ed-131">Create hello virtual machine</span></span>
<span data-ttu-id="062ed-132">Nyní spusťte toobuild konfiguraci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="062ed-132">Now start toobuild your VM configuration.</span></span> <span data-ttu-id="062ed-133">Limit velikost každého virtuálního počítače je hello celkový počet síťových adaptérů, můžete přidat tooa virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="062ed-133">Each VM size has a limit for hello total number of NICs that you can add tooa VM.</span></span> <span data-ttu-id="062ed-134">Další informace najdete v tématu [velikosti virtuálních počítačů Windows](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="062ed-134">For more information, see [Windows VM sizes](sizes.md).</span></span>

1. <span data-ttu-id="062ed-135">Nastavte přihlašovací údaje toohello váš virtuální počítač `$cred` proměnná následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="062ed-135">Set your VM credentials toohello `$cred` variable as follows:</span></span>

    ```powershell
    $cred = Get-Credential
    ```

2. <span data-ttu-id="062ed-136">Zadejte virtuální počítač s [nové AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig).</span><span class="sxs-lookup"><span data-stu-id="062ed-136">Define your VM with [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig).</span></span> <span data-ttu-id="062ed-137">Hello následující příklad definuje virtuální počítač s názvem *Můjvp* a používá velikost virtuálního počítače, který podporuje více než dva síťové adaptéry (*Standard_DS3_v2*):</span><span class="sxs-lookup"><span data-stu-id="062ed-137">hello following example defines a VM named *myVM* and uses a VM size that supports more than two NICs (*Standard_DS3_v2*):</span></span>

    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS3_v2"
    ```

3. <span data-ttu-id="062ed-138">Vytvoření hello zbytek konfiguraci virtuálních počítačů s [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) a [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage).</span><span class="sxs-lookup"><span data-stu-id="062ed-138">Create hello rest of your VM configuration with [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) and [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage).</span></span> <span data-ttu-id="062ed-139">Hello následující ukázka vytvoří virtuální počítač s Windows Server 2016:</span><span class="sxs-lookup"><span data-stu-id="062ed-139">hello following example creates a Windows Server 2016 VM:</span></span>

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

4. <span data-ttu-id="062ed-140">Připojte hello dvěma síťovými adaptéry, které jste předtím vytvořili pomocí [přidat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="062ed-140">Attach hello two NICs that you previously created with [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span></span>

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
    ```

5. <span data-ttu-id="062ed-141">Nakonec vytvořte virtuální počítač s [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="062ed-141">Finally, create your VM with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm):</span></span>

    ```powershell
    New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "EastUs"
    ```

## <a name="add-a-nic-tooan-existing-vm"></a><span data-ttu-id="062ed-142">Přidat existující virtuální počítač síťový adaptér tooan</span><span class="sxs-lookup"><span data-stu-id="062ed-142">Add a NIC tooan existing VM</span></span>
<span data-ttu-id="062ed-143">Přidat virtuální síťový adaptér tooan existující virtuální počítač, navrácení hello virtuálních počítačů, tooadd hello virtuální síťovou kartu a pak spusťte hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="062ed-143">tooadd a virtual NIC tooan existing VM, you deallocate hello VM, add hello virtual NIC, then start hello VM.</span></span>

1. <span data-ttu-id="062ed-144">Zrušit přidělení virtuálního počítače s hello [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="062ed-144">Deallocate hello VM with [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span></span> <span data-ttu-id="062ed-145">Hello následující příklad zruší přidělení hello virtuálního počítače s názvem *Můjvp* v *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="062ed-145">hello following example deallocates hello VM named *myVM* in *myResourceGroup*:</span></span>

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. <span data-ttu-id="062ed-146">Získání hello existující konfigurace hello virtuálních počítačů s [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="062ed-146">Get hello existing configuration of hello VM with [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm).</span></span> <span data-ttu-id="062ed-147">Hello následující příklad načte informace o hello virtuálního počítače s názvem *Můjvp* v *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="062ed-147">hello following example gets information for hello VM named *myVM* in *myResourceGroup*:</span></span>

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. <span data-ttu-id="062ed-148">Hello následující příklad vytvoří virtuální síťový adaptér s [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) s názvem *myNic3* připojená příliš*mySubnetBackEnd*.</span><span class="sxs-lookup"><span data-stu-id="062ed-148">hello following example creates a virtual NIC with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) named *myNic3* that is attached too*mySubnetBackEnd*.</span></span> <span data-ttu-id="062ed-149">připojit virtuální síťový adaptér je pak Hello toohello virtuálního počítače s názvem *Můjvp* v *myResourceGroup* s [přidat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="062ed-149">hello virtual NIC is then attached toohello VM named *myVM* in *myResourceGroup* with [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span></span>

    ```powershell
    # Get info for hello back end subnet
    $myVnet = Get-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName "myResourceGroup"
    $backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}

    # Create a virtual NIC
    $myNic3 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
        -Name "myNic3" `
        -Location "EastUs" `
        -SubnetId $backEnd.Id

    # Get hello ID of hello new virtual NIC and add tooVM
    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "MyNic3").Id
    Add-AzureRmVMNetworkInterface -VM $vm -Id $nicId | Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```

    ### <a name="primary-virtual-nics"></a><span data-ttu-id="062ed-150">Primární virtuální síťové karty</span><span class="sxs-lookup"><span data-stu-id="062ed-150">Primary virtual NICs</span></span>
    <span data-ttu-id="062ed-151">Jeden z hello síťové adaptéry na virtuálním počítači více síťovými Kartami, musí toobe primární.</span><span class="sxs-lookup"><span data-stu-id="062ed-151">One of hello NICs on a multi-NIC VM needs toobe primary.</span></span> <span data-ttu-id="062ed-152">Pokud jeden z hello existující virtuální síťové karty na hello virtuální počítač je již nastavena jako primární, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="062ed-152">If one of hello existing virtual NICs on hello VM is already set as primary, you can skip this step.</span></span> <span data-ttu-id="062ed-153">Hello následující příklad předpokládá, že dva virtuální síťové karty jsou teď k dispozici na virtuálním počítači a chcete tooadd hello první síťový adaptér (`[0]`) jako primární hello:</span><span class="sxs-lookup"><span data-stu-id="062ed-153">hello following example assumes that two virtual NICs are now present on a VM and you wish tooadd hello first NIC (`[0]`) as hello primary:</span></span>
        
    ```powershell
    # List existing NICs on hello VM and find which one is primary
    $vm.NetworkProfile.NetworkInterfaces
    
    # Set NIC 0 toobe primary
    $vm.NetworkProfile.NetworkInterfaces[0].Primary = $true
    $vm.NetworkProfile.NetworkInterfaces[1].Primary = $false
    
    # Update hello VM state in Azure
    Update-AzureRmVM -VM $vm -ResourceGroupName "myResourceGroup"
    ```

4. <span data-ttu-id="062ed-154">Spustit virtuální počítač s hello [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="062ed-154">Start hello VM with [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):</span></span>

    ```powershell
    Start-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

## <a name="remove-a-nic-from-an-existing-vm"></a><span data-ttu-id="062ed-155">Odeberte síťový adaptér ze stávajícího virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="062ed-155">Remove a NIC from an existing VM</span></span>
<span data-ttu-id="062ed-156">tooremove virtuální síťový adaptér ze stávajícího virtuálního počítače, můžete zrušit přidělení hello virtuálních počítačů, odeberte hello virtuální síťovou kartu a potom hello spuštění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="062ed-156">tooremove a virtual NIC from an existing VM, you deallocate hello VM, remove hello virtual NIC, then start hello VM.</span></span>

1. <span data-ttu-id="062ed-157">Zrušit přidělení virtuálního počítače s hello [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="062ed-157">Deallocate hello VM with [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span></span> <span data-ttu-id="062ed-158">Hello následující příklad zruší přidělení hello virtuálního počítače s názvem *Můjvp* v *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="062ed-158">hello following example deallocates hello VM named *myVM* in *myResourceGroup*:</span></span>

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. <span data-ttu-id="062ed-159">Získání hello existující konfigurace hello virtuálních počítačů s [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="062ed-159">Get hello existing configuration of hello VM with [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm).</span></span> <span data-ttu-id="062ed-160">Hello následující příklad načte informace o hello virtuálního počítače s názvem *Můjvp* v *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="062ed-160">hello following example gets information for hello VM named *myVM* in *myResourceGroup*:</span></span>

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. <span data-ttu-id="062ed-161">Získání informací o hello odeberte síťovou kartu s [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface).</span><span class="sxs-lookup"><span data-stu-id="062ed-161">Get information about hello NIC remove with [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface).</span></span> <span data-ttu-id="062ed-162">Hello následující příklad načte informace *myNic3*:</span><span class="sxs-lookup"><span data-stu-id="062ed-162">hello following example gets information about *myNic3*:</span></span>

    ```powershell
    # List existing NICs on hello VM if you need toodetermine NIC name
    $vm.NetworkProfile.NetworkInterfaces

    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "myNic3").Id   
    ```

4. <span data-ttu-id="062ed-163">Odebrat hello síťový adaptér s [odebrat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface) a potom aktualizovat hello virtuálního počítače s [aktualizace-AzureRmVm](/powershell/module/azurerm.compute/update-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="062ed-163">Remove hello NIC with [Remove-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface) and then update hello VM with [Update-AzureRmVm](/powershell/module/azurerm.compute/update-azurermvm).</span></span> <span data-ttu-id="062ed-164">Hello následující příklad odebere *myNic3* získány `$nicId` v předchozím kroku hello:</span><span class="sxs-lookup"><span data-stu-id="062ed-164">hello following example removes *myNic3* as obtained by `$nicId` in hello preceding step:</span></span>

    ```powershell
    Remove-AzureRmVMNetworkInterface -VM $vm -NetworkInterfaceIDs $nicId | `
        Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```   

5. <span data-ttu-id="062ed-165">Spustit virtuální počítač s hello [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="062ed-165">Start hello VM with [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):</span></span>

    ```powershell
    Start-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```   

## <a name="create-multiple-nics-with-templates"></a><span data-ttu-id="062ed-166">Vytvořit několik síťových adaptérů se šablonami</span><span class="sxs-lookup"><span data-stu-id="062ed-166">Create multiple NICs with templates</span></span>
<span data-ttu-id="062ed-167">Šablony Azure Resource Manageru poskytují způsob, jak toocreate více instancí prostředku během nasazení, jako je například vytváření několik síťových adaptérů.</span><span class="sxs-lookup"><span data-stu-id="062ed-167">Azure Resource Manager templates provide a way toocreate multiple instances of a resource during deployment, such as creating multiple NICs.</span></span> <span data-ttu-id="062ed-168">Šablony Resource Manageru pomocí deklarativní toodefine soubory JSON prostředí.</span><span class="sxs-lookup"><span data-stu-id="062ed-168">Resource Manager templates use declarative JSON files toodefine your environment.</span></span> <span data-ttu-id="062ed-169">Další informace najdete v tématu [přehled nástroje Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="062ed-169">For more information, see [overview of Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="062ed-170">Můžete použít *kopie* toospecify hello počet instancí toocreate:</span><span class="sxs-lookup"><span data-stu-id="062ed-170">You can use *copy* toospecify hello number of instances toocreate:</span></span>

```json
"copy": {
    "name": "multiplenics",
    "count": "[parameters('count')]"
}
```

<span data-ttu-id="062ed-171">Další informace najdete v tématu [vytvoření více instancí pomocí *kopie*](../../resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="062ed-171">For more information, see [creating multiple instances by using *copy*](../../resource-group-create-multiple.md).</span></span> 

<span data-ttu-id="062ed-172">Můžete také použít `copyIndex()` tooappend číslo tooa název prostředku.</span><span class="sxs-lookup"><span data-stu-id="062ed-172">You can also use `copyIndex()` tooappend a number tooa resource name.</span></span> <span data-ttu-id="062ed-173">Potom můžete vytvořit *myNic1*, *MyNic2* a tak dále.</span><span class="sxs-lookup"><span data-stu-id="062ed-173">You can then create *myNic1*, *MyNic2* and so on.</span></span> <span data-ttu-id="062ed-174">Hello následující kód ukazuje příklad připojování hodnotu indexu hello:</span><span class="sxs-lookup"><span data-stu-id="062ed-174">hello following code shows an example of appending hello index value:</span></span>

```json
"name": "[concat('myNic', copyIndex())]", 
```

<span data-ttu-id="062ed-175">Kompletní příklad, jak si můžete přečíst [vytvoření několik síťových adaptérů pomocí šablony Resource Manageru](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="062ed-175">You can read a complete example of [creating multiple NICs by using Resource Manager templates](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="062ed-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="062ed-176">Next steps</span></span>
<span data-ttu-id="062ed-177">Zkontrolujte [velikosti virtuálních počítačů Windows](sizes.md) když se snažíte toocreate virtuální počítač, který má několik síťových adaptérů.</span><span class="sxs-lookup"><span data-stu-id="062ed-177">Review [Windows VM sizes](sizes.md) when you're trying toocreate a VM that has multiple NICs.</span></span> <span data-ttu-id="062ed-178">Věnujte pozornost toohello maximální počet síťových adaptérů, které podporuje každý velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="062ed-178">Pay attention toohello maximum number of NICs that each VM size supports.</span></span> 


