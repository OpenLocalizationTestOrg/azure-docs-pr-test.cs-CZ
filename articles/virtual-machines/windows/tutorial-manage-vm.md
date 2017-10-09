---
title: "aaaCreate a spravovat virtuální počítače Windows s hello modul Azure PowerShell | Microsoft Docs"
description: "Kurz – vytvoření a správa virtuálních počítačů Windows s hello modul Azure PowerShell"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 20adcb673ef4de683e6ad82d048a9625a1dc838c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-windows-vms-with-hello-azure-powershell-module"></a><span data-ttu-id="c23f0-103">Vytvoření a správa virtuálních počítačů Windows s hello modul Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c23f0-103">Create and Manage Windows VMs with hello Azure PowerShell module</span></span>

<span data-ttu-id="c23f0-104">Virtuální počítače Azure, zadejte plně konfigurovatelné a flexibilní výpočetního prostředí.</span><span class="sxs-lookup"><span data-stu-id="c23f0-104">Azure virtual machines provide a fully configurable and flexible computing environment.</span></span> <span data-ttu-id="c23f0-105">Tento kurz se zaměřuje na základní virtuální počítač Azure nasazení položky například vyberete velikost virtuálního počítače, výběr image virtuálního počítače a nasazení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c23f0-105">This tutorial covers basic Azure virtual machine deployment items such as selecting a VM size, selecting a VM image, and deploying a VM.</span></span> <span data-ttu-id="c23f0-106">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="c23f0-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c23f0-107">Vytvořit a připojit tooa virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="c23f0-107">Create and connect tooa VM</span></span>
> * <span data-ttu-id="c23f0-108">Vyberte a použijte Image virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="c23f0-108">Select and use VM images</span></span>
> * <span data-ttu-id="c23f0-109">Zobrazení a používání určité velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="c23f0-109">View and use specific VM sizes</span></span>
> * <span data-ttu-id="c23f0-110">Změna velikosti virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="c23f0-110">Resize a VM</span></span>
> * <span data-ttu-id="c23f0-111">Zobrazení a pochopit stav virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="c23f0-111">View and understand VM state</span></span>

<span data-ttu-id="c23f0-112">Tento kurz vyžaduje hello prostředí Azure PowerShell verze modulu 3,6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="c23f0-112">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="c23f0-113">Spustit ` Get-Module -ListAvailable AzureRM` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="c23f0-113">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="c23f0-114">Pokud potřebujete tooupgrade, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="c23f0-114">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="create-resource-group"></a><span data-ttu-id="c23f0-115">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="c23f0-115">Create resource group</span></span>

<span data-ttu-id="c23f0-116">Vytvořte skupinu prostředků s hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) příkaz.</span><span class="sxs-lookup"><span data-stu-id="c23f0-116">Create a resource group with hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) command.</span></span> 

<span data-ttu-id="c23f0-117">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="c23f0-117">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="c23f0-118">Před virtuálního počítače je třeba vytvořit skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="c23f0-118">A resource group must be created before a virtual machine.</span></span> <span data-ttu-id="c23f0-119">V tomto příkladu skupinu prostředků s názvem *myResourceGroupVM* je vytvořen v hello *EastUS* oblast.</span><span class="sxs-lookup"><span data-stu-id="c23f0-119">In this example, a resource group named *myResourceGroupVM* is created in hello *EastUS* region.</span></span> 

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupVM -Location EastUS
```

<span data-ttu-id="c23f0-120">Skupina prostředků Hello je zadána při vytváření nebo úpravách virtuálního počítače, které jsou viditelné v rámci tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="c23f0-120">hello resource group is specified when creating or modifying a VM, which can be seen throughout this tutorial.</span></span>

## <a name="create-virtual-machine"></a><span data-ttu-id="c23f0-121">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="c23f0-121">Create virtual machine</span></span>

<span data-ttu-id="c23f0-122">Virtuální počítač musí být připojené tooa virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="c23f0-122">A virtual machine must be connected tooa virtual network.</span></span> <span data-ttu-id="c23f0-123">Byste komunikovat s hello virtuálního počítače pomocí veřejnou IP adresu prostřednictvím karty síťového rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c23f0-123">You communicate with hello virtual machine using a public IP address through a network interface card.</span></span>

### <a name="create-virtual-network"></a><span data-ttu-id="c23f0-124">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="c23f0-124">Create virtual network</span></span>

<span data-ttu-id="c23f0-125">Vytvořte podsíť s [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="c23f0-125">Create a subnet with [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):</span></span>

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
```

<span data-ttu-id="c23f0-126">Vytvoření virtuální sítě s [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="c23f0-126">Create a virtual network with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 ` 
  -Subnet $subnetConfig
```
### <a name="create-public-ip-address"></a><span data-ttu-id="c23f0-127">Vytvoření veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="c23f0-127">Create public IP address</span></span>

<span data-ttu-id="c23f0-128">Vytvoření veřejné IP adresy s [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):</span><span class="sxs-lookup"><span data-stu-id="c23f0-128">Create a public IP address with [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):</span></span>

```powershell
$pip = New-AzureRmPublicIpAddress ` 
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS ` 
  -AllocationMethod Static `
  -Name myPublicIPAddress
```

### <a name="create-network-interface-card"></a><span data-ttu-id="c23f0-129">Vytvoření karty síťového rozhraní</span><span class="sxs-lookup"><span data-stu-id="c23f0-129">Create network interface card</span></span>

<span data-ttu-id="c23f0-130">Vytvoření síťová karta s [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="c23f0-130">Create a network interface card with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):</span></span>

```powershell
$nic = New-AzureRmNetworkInterface `
  -ResourceGroupName myResourceGroupVM  `
  -Location EastUS `
  -Name myNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

### <a name="create-network-security-group"></a><span data-ttu-id="c23f0-131">Vytvořit skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="c23f0-131">Create network security group</span></span>

<span data-ttu-id="c23f0-132">Azure [skupinu zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md) (NSG) řídí příchozí a odchozí přenosy pro jeden nebo více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c23f0-132">An Azure [network security group](../../virtual-network/virtual-networks-nsg.md) (NSG) controls inbound and outbound traffic for one or many virtual machines.</span></span> <span data-ttu-id="c23f0-133">Pravidla skupiny zabezpečení sítě, povolit nebo odepřít síťový provoz na konkrétní port nebo rozsah portů.</span><span class="sxs-lookup"><span data-stu-id="c23f0-133">Network security group rules allow or deny network traffic on a specific port or port range.</span></span> <span data-ttu-id="c23f0-134">Tato pravidla může zahrnovat předpona zdrojové adresy tak, aby jenom přenosy v předdefinované zdroj může komunikovat s virtuálním počítačem.</span><span class="sxs-lookup"><span data-stu-id="c23f0-134">These rules can also include a source address prefix so that only traffic originating at a predefined source can communicate with a virtual machine.</span></span> <span data-ttu-id="c23f0-135">tooaccess hello IIS webový server, kterou instalujete, je nutné přidat příchozí pravidlo NSG.</span><span class="sxs-lookup"><span data-stu-id="c23f0-135">tooaccess hello IIS webserver that you are installing, you must add an inbound NSG rule.</span></span>

<span data-ttu-id="c23f0-136">použít toocreate příchozího pravidla NSG, [přidat AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig).</span><span class="sxs-lookup"><span data-stu-id="c23f0-136">toocreate an inbound NSG rule, use [Add-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig).</span></span> <span data-ttu-id="c23f0-137">Hello následující příklad vytvoří pravidlo NSG s názvem *myNSGRule* , otevře port *3389* hello virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="c23f0-137">hello following example creates an NSG rule named *myNSGRule* that opens port *3389* for hello virtual machine:</span></span>

```powershell
$nsgRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1000 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 3389 `
  -Access Allow
```

<span data-ttu-id="c23f0-138">Vytvořte pomocí NSG hello *myNSGRule* s [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):</span><span class="sxs-lookup"><span data-stu-id="c23f0-138">Create hello NSG using *myNSGRule* with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):</span></span>

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupVM `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRule
```

<span data-ttu-id="c23f0-139">Přidat hello NSG toohello podsíť ve virtuální síti hello s [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="c23f0-139">Add hello NSG toohello subnet in hello virtual network with [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):</span></span>

```powershell
Set-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -VirtualNetwork $vnet `
    -NetworkSecurityGroup $nsg `
    -AddressPrefix 192.168.1.0/24
```

<span data-ttu-id="c23f0-140">Aktualizace hello virtuální síť s [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="c23f0-140">Update hello virtual network with [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):</span></span>

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-machine"></a><span data-ttu-id="c23f0-141">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="c23f0-141">Create virtual machine</span></span>

<span data-ttu-id="c23f0-142">Při vytváření virtuálního počítače, jsou k dispozici jako jsou bitové kopie operačního systému, přihlašovací údaje velikost a správu disku několik možností.</span><span class="sxs-lookup"><span data-stu-id="c23f0-142">When creating a virtual machine, several options are available such as operating system image, disk sizing, and administrative credentials.</span></span> <span data-ttu-id="c23f0-143">V tomto příkladu se vytvoří virtuální počítač s názvem *Můjvp* spuštěné hello nejnovější verzi systému Windows Server 2016 Datacenter.</span><span class="sxs-lookup"><span data-stu-id="c23f0-143">In this example, a virtual machine is created with a name of *myVM* running hello latest version of Windows Server 2016 Datacenter.</span></span>

<span data-ttu-id="c23f0-144">Nastavte hello uživatelské jméno a heslo, které potřebuje pro účet správce hello na hello virtuální počítač s [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="c23f0-144">Set hello username and password needed for hello administrator account on hello virtual machine with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="c23f0-145">Vytvořit hello počáteční konfiguraci pro virtuální počítač hello s [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig):</span><span class="sxs-lookup"><span data-stu-id="c23f0-145">Create hello initial configuration for hello virtual machine with [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig):</span></span>

```powershell
$vm = New-AzureRmVMConfig -VMName myVM -VMSize Standard_D1
```

<span data-ttu-id="c23f0-146">Přidat konfiguraci virtuálního počítače hello operačního systému informace toohello s [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem):</span><span class="sxs-lookup"><span data-stu-id="c23f0-146">Add hello operating system information toohello virtual machine configuration with [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem):</span></span>

```powershell
$vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM `
    -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
```

<span data-ttu-id="c23f0-147">Přidat konfiguraci virtuálního počítače hello image informace toohello s [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage):</span><span class="sxs-lookup"><span data-stu-id="c23f0-147">Add hello image information toohello virtual machine configuration with [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage):</span></span>

```powershell
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
```

<span data-ttu-id="c23f0-148">Přidat hello konfigurace operačního systému disku nastavení toohello virtuální počítač s [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk):</span><span class="sxs-lookup"><span data-stu-id="c23f0-148">Add hello operating system disk settings toohello virtual machine configuration with [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk):</span></span>

```powershell
$vm = Set-AzureRmVMOSDisk `
    -VM $vm `
    -Name myOsDisk `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
```

<span data-ttu-id="c23f0-149">Přidat hello síťové rozhraní karty, který jste dříve vytvořili toohello konfiguraci virtuálního počítače s [přidat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="c23f0-149">Add hello network interface card that you previously created toohello virtual machine configuration with [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span></span>

```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

<span data-ttu-id="c23f0-150">Vytvoření virtuálního počítače hello s [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="c23f0-150">Create hello virtual machine with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span>

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroupVM -Location EastUS -VM $vm
```

## <a name="connect-toovm"></a><span data-ttu-id="c23f0-151">Připojit tooVM</span><span class="sxs-lookup"><span data-stu-id="c23f0-151">Connect tooVM</span></span>

<span data-ttu-id="c23f0-152">Po dokončení nasazení hello vytvořte připojení ke vzdálené ploše se hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c23f0-152">After hello deployment has completed, create a remote desktop connection with hello virtual machine.</span></span>

<span data-ttu-id="c23f0-153">Spusťte následující příkazy tooreturn hello veřejnou IP adresu virtuálního počítače hello hello.</span><span class="sxs-lookup"><span data-stu-id="c23f0-153">Run hello following commands tooreturn hello public IP address of hello virtual machine.</span></span> <span data-ttu-id="c23f0-154">Poznamenejte si tuto IP adresu, tooit můžete propojit s váš prohlížeč tootest připojení k webu v příštím kroku.</span><span class="sxs-lookup"><span data-stu-id="c23f0-154">Take note of this IP Address so you can connect tooit with your browser tootest web connectivity in a future step.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroupVM  | Select IpAddress
```

<span data-ttu-id="c23f0-155">Použití hello následující příkaz toocreate a relace vzdálené plochy s hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c23f0-155">Use hello following command toocreate a remote desktop session with hello virtual machine.</span></span> <span data-ttu-id="c23f0-156">Nahraďte IP adresu hello hello *publicIPAddress* virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c23f0-156">Replace hello IP address with hello *publicIPAddress* of your virtual machine.</span></span> <span data-ttu-id="c23f0-157">Po zobrazení výzvy zadejte přihlašovací údaje hello použité při vytváření hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c23f0-157">When prompted, enter hello credentials used when creating hello virtual machine.</span></span>

```powershell
mstsc /v:<publicIpAddress>
```

## <a name="understand-vm-images"></a><span data-ttu-id="c23f0-158">Pochopení Image virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="c23f0-158">Understand VM images</span></span>

<span data-ttu-id="c23f0-159">Hello Azure marketplace obsahuje mnoho bitové kopie virtuálních počítačů, které se dají použít toocreate nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c23f0-159">hello Azure marketplace includes many virtual machine images that can be used toocreate a new virtual machine.</span></span> <span data-ttu-id="c23f0-160">V předchozích krocích hello byl vytvořen virtuální počítač pomocí bitové kopie systému Windows Server 2016-Datacenter hello.</span><span class="sxs-lookup"><span data-stu-id="c23f0-160">In hello previous steps, a virtual machine was created using hello Windows Server 2016-Datacenter image.</span></span> <span data-ttu-id="c23f0-161">V tomto kroku modul prostředí PowerShell hello je použité toosearch hello marketplace pro jiné Image systému Windows, které může taky jako základ pro nové virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="c23f0-161">In this step, hello PowerShell module is used toosearch hello marketplace for other Windows images, which can also as a base for new VMs.</span></span> <span data-ttu-id="c23f0-162">Tento proces se skládá z hledání hello vydavatele, nabídky a název bitové kopie hello (Sku).</span><span class="sxs-lookup"><span data-stu-id="c23f0-162">This process consists of finding hello publisher, offer, and hello image name (Sku).</span></span> 

<span data-ttu-id="c23f0-163">Použití hello [Get-AzureRmVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher) tooreturn příkaz seznam vydavatelů bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="c23f0-163">Use hello [Get-AzureRmVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher) command tooreturn a list of image publishers.</span></span>  

```powersehll
Get-AzureRmVMImagePublisher -Location "EastUS"
```

<span data-ttu-id="c23f0-164">Použití hello [Get-AzureRmVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) tooreturn seznam nabídek bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="c23f0-164">Use hello [Get-AzureRmVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) tooreturn a list of image offers.</span></span> <span data-ttu-id="c23f0-165">Pomocí tohoto příkazu hello vrátit seznam je filtrovaný na zadaný vydavatel hello.</span><span class="sxs-lookup"><span data-stu-id="c23f0-165">With this command, hello returned list is filtered on hello specified publisher.</span></span> 

```powershell
Get-AzureRmVMImageOffer -Location "EastUS" -PublisherName "MicrosoftWindowsServer"
```

```powershell
Offer             PublisherName          Location
-----             -------------          -------- 
Windows-HUB       MicrosoftWindowsServer EastUS 
WindowsServer     MicrosoftWindowsServer EastUS   
WindowsServer-HUB MicrosoftWindowsServer EastUS   
```

<span data-ttu-id="c23f0-166">Hello [Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) příkaz bude potom filtrovat tooreturn název vydavatele a nabídka hello seznam názvů bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="c23f0-166">hello [Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) command will then filter on hello publisher and offer name tooreturn a list of image names.</span></span>

```powershell
Get-AzureRmVMImageSku -Location "EastUS" -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer"
```

```powershell
Skus                            Offer         PublisherName          Location
----                            -----         -------------          --------
2008-R2-SP1                     WindowsServer MicrosoftWindowsServer EastUS  
2008-R2-SP1-BYOL                WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter                 WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter-BYOL            WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter              WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter-BYOL         WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter                 WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-Server-Core     WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-with-Containers WindowsServer MicrosoftWindowsServer EastUS  
2016-Nano-Server                WindowsServer MicrosoftWindowsServer EastUS
```

<span data-ttu-id="c23f0-167">Tato informace může být použité toodeploy virtuální počítač s konkrétní image.</span><span class="sxs-lookup"><span data-stu-id="c23f0-167">This information can be used toodeploy a VM with a specific image.</span></span> <span data-ttu-id="c23f0-168">Tento příklad nastaví název bitové kopie hello hello objekt virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c23f0-168">This example sets hello image name on hello VM object.</span></span> <span data-ttu-id="c23f0-169">V tomto kurzu postup dokončení nasazení naleznete v předchozích příkladech toohello.</span><span class="sxs-lookup"><span data-stu-id="c23f0-169">Refer toohello previous examples in this tutorial for complete deployment steps.</span></span>

```powershell
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter-with-Containers `
    -Version latest
```

## <a name="understand-vm-sizes"></a><span data-ttu-id="c23f0-170">Pochopení velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="c23f0-170">Understand VM sizes</span></span>

<span data-ttu-id="c23f0-171">Velikost virtuálního počítače určuje hello množství výpočetní prostředky, například procesoru, grafického procesoru a paměti, které jsou vytvářeny k dispozici toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c23f0-171">A virtual machine size determines hello amount of compute resources such as CPU, GPU, and memory that are made available toohello virtual machine.</span></span> <span data-ttu-id="c23f0-172">Třeba virtuální počítače toobe vytvořené s velikostí vhodné pro hello očekávat pracovní zatížení.</span><span class="sxs-lookup"><span data-stu-id="c23f0-172">Virtual machines need toobe created with a size appropriate for hello expect work load.</span></span> <span data-ttu-id="c23f0-173">Hodnota se zvyšuje zatížení, můžete ke změně velikosti existujícího virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c23f0-173">If workload increases, an existing virtual machine can be resized.</span></span>

### <a name="vm-sizes"></a><span data-ttu-id="c23f0-174">Velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="c23f0-174">VM Sizes</span></span>

<span data-ttu-id="c23f0-175">Následující tabulka Hello rozděluje velikosti do případy použití.</span><span class="sxs-lookup"><span data-stu-id="c23f0-175">hello following table categorizes sizes into use cases.</span></span>  

| <span data-ttu-id="c23f0-176">Typ</span><span class="sxs-lookup"><span data-stu-id="c23f0-176">Type</span></span>                     | <span data-ttu-id="c23f0-177">Velikosti</span><span class="sxs-lookup"><span data-stu-id="c23f0-177">Sizes</span></span>           |    <span data-ttu-id="c23f0-178">Popis</span><span class="sxs-lookup"><span data-stu-id="c23f0-178">Description</span></span>       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c23f0-179">Obecné účely</span><span class="sxs-lookup"><span data-stu-id="c23f0-179">General purpose</span></span>         |<span data-ttu-id="c23f0-180">DSv2, Dv2, DS, D, Av2, A0 7</span><span class="sxs-lookup"><span data-stu-id="c23f0-180">DSv2, Dv2, DS, D, Av2, A0-7</span></span>| <span data-ttu-id="c23f0-181">Vyrovnáváním procesoru do paměti.</span><span class="sxs-lookup"><span data-stu-id="c23f0-181">Balanced CPU-to-memory.</span></span> <span data-ttu-id="c23f0-182">Ideální pro vývojáře nebo test a řešení pro malé toomedium aplikacím a datům.</span><span class="sxs-lookup"><span data-stu-id="c23f0-182">Ideal for dev / test and small toomedium applications and data solutions.</span></span>  |
| <span data-ttu-id="c23f0-183">Optimalizované z hlediska výpočetních služeb</span><span class="sxs-lookup"><span data-stu-id="c23f0-183">Compute optimized</span></span>      | <span data-ttu-id="c23f0-184">Služby FS, F</span><span class="sxs-lookup"><span data-stu-id="c23f0-184">Fs, F</span></span>             | <span data-ttu-id="c23f0-185">Vysoké využití procesoru do paměti.</span><span class="sxs-lookup"><span data-stu-id="c23f0-185">High CPU-to-memory.</span></span> <span data-ttu-id="c23f0-186">Je vhodný pro střední provoz aplikace, síťových zařízení a dávkových procesů.</span><span class="sxs-lookup"><span data-stu-id="c23f0-186">Good for medium traffic applications, network appliances, and batch processes.</span></span>        |
| <span data-ttu-id="c23f0-187">Optimalizované z hlediska paměti</span><span class="sxs-lookup"><span data-stu-id="c23f0-187">Memory optimized</span></span>       | <span data-ttu-id="c23f0-188">GS, G, DSv2, DS, Dv2, D</span><span class="sxs-lookup"><span data-stu-id="c23f0-188">GS, G, DSv2, DS, Dv2, D</span></span>   | <span data-ttu-id="c23f0-189">Vysoká paměti na core.</span><span class="sxs-lookup"><span data-stu-id="c23f0-189">High memory-to-core.</span></span> <span data-ttu-id="c23f0-190">Výborně hodí pro relačních databází, střední toolarge mezipaměti a analýzy v paměti.</span><span class="sxs-lookup"><span data-stu-id="c23f0-190">Great for relational databases, medium toolarge caches, and in-memory analytics.</span></span>                 |
| <span data-ttu-id="c23f0-191">Optimalizované z hlediska úložiště</span><span class="sxs-lookup"><span data-stu-id="c23f0-191">Storage optimized</span></span>       | <span data-ttu-id="c23f0-192">Ls</span><span class="sxs-lookup"><span data-stu-id="c23f0-192">Ls</span></span>                | <span data-ttu-id="c23f0-193">Vysoká propustnost disku a V/V.</span><span class="sxs-lookup"><span data-stu-id="c23f0-193">High disk throughput and IO.</span></span> <span data-ttu-id="c23f0-194">Ideální pro databáze NoSQL, SQL a velké objemy dat.</span><span class="sxs-lookup"><span data-stu-id="c23f0-194">Ideal for Big Data, SQL, and NoSQL databases.</span></span>                                                         |
| <span data-ttu-id="c23f0-195">GPU</span><span class="sxs-lookup"><span data-stu-id="c23f0-195">GPU</span></span>           | <span data-ttu-id="c23f0-196">VS, NC</span><span class="sxs-lookup"><span data-stu-id="c23f0-196">NV, NC</span></span>            | <span data-ttu-id="c23f0-197">Specializované virtuální počítače cílené pro velkou grafické vykreslování a úpravy videa.</span><span class="sxs-lookup"><span data-stu-id="c23f0-197">Specialized VMs targeted for heavy graphic rendering and video editing.</span></span>       |
| <span data-ttu-id="c23f0-198">Vysoký výkon</span><span class="sxs-lookup"><span data-stu-id="c23f0-198">High performance</span></span> | <span data-ttu-id="c23f0-199">H, A8 11</span><span class="sxs-lookup"><span data-stu-id="c23f0-199">H, A8-11</span></span>          | <span data-ttu-id="c23f0-200">Naše nejúčinnějších procesoru virtuálních počítačů s volitelné vysokou propustností síťová rozhraní (počítače RDMA).</span><span class="sxs-lookup"><span data-stu-id="c23f0-200">Our most powerful CPU VMs with optional high-throughput network interfaces (RDMA).</span></span> 


### <a name="find-available-vm-sizes"></a><span data-ttu-id="c23f0-201">Najít dostupných velikostí virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="c23f0-201">Find available VM sizes</span></span>

<span data-ttu-id="c23f0-202">toosee seznam virtuálních počítačů velikostí k dispozici v určité oblasti, použijte hello [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) příkaz.</span><span class="sxs-lookup"><span data-stu-id="c23f0-202">toosee a list of VM sizes available in a particular region, use hello [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) command.</span></span>

```powershell
Get-AzureRmVMSize -Location EastUS
```

## <a name="resize-a-vm"></a><span data-ttu-id="c23f0-203">Změna velikosti virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="c23f0-203">Resize a VM</span></span>

<span data-ttu-id="c23f0-204">Po nasazení virtuálního počítače, může být změněnou tooincrease nebo snížit přidělení prostředků.</span><span class="sxs-lookup"><span data-stu-id="c23f0-204">After a VM has been deployed, it can be resized tooincrease or decrease resource allocation.</span></span>

<span data-ttu-id="c23f0-205">Před změnou velikosti virtuálního počítače, zkontrolujte, zda hello požadovaná velikost je k dispozici na hello aktuální cluster virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c23f0-205">Before resizing a VM, check if hello desired size is available on hello current VM cluster.</span></span> <span data-ttu-id="c23f0-206">Hello [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) příkaz vrátí seznam velikostí.</span><span class="sxs-lookup"><span data-stu-id="c23f0-206">hello [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) command returns a list of sizes.</span></span> 

```powershell
Get-AzureRmVMSize -ResourceGroupName myResourceGroupVM -VMName myVM 
```

<span data-ttu-id="c23f0-207">V případě, že velikost je k dispozici potřeby hello hello virtuálních počítačů velikost lze změnit ze stavu na zapnuté, ale po restartu během operace hello.</span><span class="sxs-lookup"><span data-stu-id="c23f0-207">If hello desired size is available, hello VM can be resized from a powered-on state, however it is rebooted during hello operation.</span></span>

```powershell
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM 
$vm.HardwareProfile.VmSize = "Standard_D4"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
```

<span data-ttu-id="c23f0-208">V případě potřeby hello velikost není v aktuální clusteru hello hello virtuálních počítačů potřeb, které může dojít toobe navrácena před hello změnit velikost operace.</span><span class="sxs-lookup"><span data-stu-id="c23f0-208">If hello desired size is not on hello current cluster, hello VM needs toobe deallocated before hello resize operation can occur.</span></span> <span data-ttu-id="c23f0-209">Poznámka: když hello virtuální počítač je zapnutý zpět, se odeberou všechna data na dočasné disku hello a hello veřejnou IP adresu změnit, dokud se používá statickou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="c23f0-209">Note, when hello VM is powered back on, any data on hello temp disk are removed, and hello public IP address change unless a static IP address is being used.</span></span> 

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM
$vm.HardwareProfile.VmSize = "Standard_F4s"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
Start-AzureRmVM -ResourceGroupName myResourceGroupVM  -Name $vm.name
```

## <a name="vm-power-states"></a><span data-ttu-id="c23f0-210">Stavy spotřeby virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="c23f0-210">VM power states</span></span>

<span data-ttu-id="c23f0-211">Virtuální počítač Azure může mít jednu z mnoha snížené spotřeby energie.</span><span class="sxs-lookup"><span data-stu-id="c23f0-211">An Azure VM can have one of many power states.</span></span> <span data-ttu-id="c23f0-212">Tento stav představuje hello aktuální stav hello virtuálních počítačů z hlediska hello hello hypervisoru.</span><span class="sxs-lookup"><span data-stu-id="c23f0-212">This state represents hello current state of hello VM from hello standpoint of hello hypervisor.</span></span> 

### <a name="power-states"></a><span data-ttu-id="c23f0-213">Stavy spotřeby.</span><span class="sxs-lookup"><span data-stu-id="c23f0-213">Power states</span></span>

| <span data-ttu-id="c23f0-214">Stav napájení</span><span class="sxs-lookup"><span data-stu-id="c23f0-214">Power State</span></span> | <span data-ttu-id="c23f0-215">Popis</span><span class="sxs-lookup"><span data-stu-id="c23f0-215">Description</span></span>
|----|----|
| <span data-ttu-id="c23f0-216">Spouštění</span><span class="sxs-lookup"><span data-stu-id="c23f0-216">Starting</span></span> | <span data-ttu-id="c23f0-217">Označuje, že se spouští hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c23f0-217">Indicates hello virtual machine is being started.</span></span> |
| <span data-ttu-id="c23f0-218">Běžící (Spuštěno)</span><span class="sxs-lookup"><span data-stu-id="c23f0-218">Running</span></span> | <span data-ttu-id="c23f0-219">Označuje, že aby hello virtuální počítač běží.</span><span class="sxs-lookup"><span data-stu-id="c23f0-219">Indicates that hello virtual machine is running.</span></span> |
| <span data-ttu-id="c23f0-220">Zastavování</span><span class="sxs-lookup"><span data-stu-id="c23f0-220">Stopping</span></span> | <span data-ttu-id="c23f0-221">Označuje, že Probíhá zastavování virtuálního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="c23f0-221">Indicates that hello virtual machine is being stopped.</span></span> | 
| <span data-ttu-id="c23f0-222">Zastaveno</span><span class="sxs-lookup"><span data-stu-id="c23f0-222">Stopped</span></span> | <span data-ttu-id="c23f0-223">Označuje, že tento virtuální počítač hello je zastavena.</span><span class="sxs-lookup"><span data-stu-id="c23f0-223">Indicates that hello virtual machine is stopped.</span></span> <span data-ttu-id="c23f0-224">Všimněte si, že virtuální počítače ve stavu Zastaveno hello stále platit poplatky výpočty.</span><span class="sxs-lookup"><span data-stu-id="c23f0-224">Note that virtual machines in hello stopped state still incur compute charges.</span></span>  |
| <span data-ttu-id="c23f0-225">Rušení přidělování</span><span class="sxs-lookup"><span data-stu-id="c23f0-225">Deallocating</span></span> | <span data-ttu-id="c23f0-226">Označuje, že tento virtuální počítač hello je navrácena.</span><span class="sxs-lookup"><span data-stu-id="c23f0-226">Indicates that hello virtual machine is being deallocated.</span></span> |
| <span data-ttu-id="c23f0-227">Zrušeno</span><span class="sxs-lookup"><span data-stu-id="c23f0-227">Deallocated</span></span> | <span data-ttu-id="c23f0-228">Označuje, že tento virtuální počítač hello je úplně odebrat z hello hypervisoru, ale stále k dispozici v rovině řízení hello.</span><span class="sxs-lookup"><span data-stu-id="c23f0-228">Indicates that hello virtual machine is completely removed from hello hypervisor but still available in hello control plane.</span></span> <span data-ttu-id="c23f0-229">Virtuální počítače v hello Deallocated stavu nevznikají poplatky za výpočty.</span><span class="sxs-lookup"><span data-stu-id="c23f0-229">Virtual machines in hello Deallocated state do not incur compute charges.</span></span> |
| - | <span data-ttu-id="c23f0-230">Označuje, že stav napájení hello hello virtuálního počítače neznámý.</span><span class="sxs-lookup"><span data-stu-id="c23f0-230">Indicates that hello power state of hello virtual machine is unknown.</span></span> |

### <a name="find-power-state"></a><span data-ttu-id="c23f0-231">Najít stav napájení</span><span class="sxs-lookup"><span data-stu-id="c23f0-231">Find power state</span></span>

<span data-ttu-id="c23f0-232">Stav hello tooretrieve konkrétní virtuální počítač, použijte hello [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) příkaz.</span><span class="sxs-lookup"><span data-stu-id="c23f0-232">tooretrieve hello state of a particular VM, use hello [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) command.</span></span> <span data-ttu-id="c23f0-233">Zda toospecify být platný název pro virtuální počítač a skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="c23f0-233">Be sure toospecify a valid name for a virtual machine and resource group.</span></span> 

```powershell
Get-AzureRmVM `
    -ResourceGroupName myResourceGroup `
    -Name myVM `
    -Status | Select @{n="Status"; e={$_.Statuses[1].Code}}
```

<span data-ttu-id="c23f0-234">Výstup:</span><span class="sxs-lookup"><span data-stu-id="c23f0-234">Output:</span></span>

```powershell
Status
------
PowerState/running
```

## <a name="management-tasks"></a><span data-ttu-id="c23f0-235">Úlohy správy</span><span class="sxs-lookup"><span data-stu-id="c23f0-235">Management tasks</span></span>

<span data-ttu-id="c23f0-236">Během životního cyklu hello virtuálního počítače můžete úlohy správy toorun například spuštění, zastavení nebo odstranění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c23f0-236">During hello lifecycle of a virtual machine, you may want toorun management tasks such as starting, stopping, or deleting a virtual machine.</span></span> <span data-ttu-id="c23f0-237">Kromě toho můžete toocreate skripty tooautomate opakovaných nebo komplexní úlohy.</span><span class="sxs-lookup"><span data-stu-id="c23f0-237">Additionally, you may want toocreate scripts tooautomate repetitive or complex tasks.</span></span> <span data-ttu-id="c23f0-238">Pomocí Azure PowerShell, mnoho běžné úlohy správy lze spustit z příkazového řádku hello nebo ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="c23f0-238">Using Azure PowerShell, many common management tasks can be run from hello command line or in scripts.</span></span>

### <a name="stop-virtual-machine"></a><span data-ttu-id="c23f0-239">Zastavit virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="c23f0-239">Stop virtual machine</span></span>

<span data-ttu-id="c23f0-240">Zastavení a zrušit přidělení virtuálního počítače s [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="c23f0-240">Stop and deallocate a virtual machine with [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm):</span></span>

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
```

<span data-ttu-id="c23f0-241">Pokud chcete tookeep hello virtuální počítač ve stavu, zřízený, použijte parametr - StayProvisioned hello.</span><span class="sxs-lookup"><span data-stu-id="c23f0-241">If you want tookeep hello virtual machine in a provisioned state, use hello -StayProvisioned parameter.</span></span>

### <a name="start-virtual-machine"></a><span data-ttu-id="c23f0-242">Spustit virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="c23f0-242">Start virtual machine</span></span>

```powershell
Start-AzureRmVM -ResourceGroupName myResourceGroupVM -Name myVM
```

### <a name="delete-resource-group"></a><span data-ttu-id="c23f0-243">Odstraňte skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="c23f0-243">Delete resource group</span></span>

<span data-ttu-id="c23f0-244">Odstranění skupiny prostředků se také odstraní všechny prostředky obsažené v rámci.</span><span class="sxs-lookup"><span data-stu-id="c23f0-244">Deleting a resource group also deletes all resources contained within.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroupVM -Force
```

## <a name="next-steps"></a><span data-ttu-id="c23f0-245">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c23f0-245">Next steps</span></span>

<span data-ttu-id="c23f0-246">V tomto kurzu jste se dozvěděli o základní vytvoření virtuálního počítače a správy, jako například:</span><span class="sxs-lookup"><span data-stu-id="c23f0-246">In this tutorial, you learned about basic VM creation and management such as how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c23f0-247">Vytvořit a připojit tooa virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="c23f0-247">Create and connect tooa VM</span></span>
> * <span data-ttu-id="c23f0-248">Vyberte a použijte Image virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="c23f0-248">Select and use VM images</span></span>
> * <span data-ttu-id="c23f0-249">Zobrazení a používání určité velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="c23f0-249">View and use specific VM sizes</span></span>
> * <span data-ttu-id="c23f0-250">Změna velikosti virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="c23f0-250">Resize a VM</span></span>
> * <span data-ttu-id="c23f0-251">Zobrazení a pochopit stav virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="c23f0-251">View and understand VM state</span></span>

<span data-ttu-id="c23f0-252">Posunutí další kurz toolearn toohello o disky virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c23f0-252">Advance toohello next tutorial toolearn about VM disks.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="c23f0-253">Vytvoření a správa virtuálních počítačů disky</span><span class="sxs-lookup"><span data-stu-id="c23f0-253">Create and Manage VM disks</span></span>](./tutorial-manage-data-disk.md)
