---
title: "Vytvoření a správa virtuálních počítačů Windows s modulu Azure PowerShell | Microsoft Docs"
description: "Kurz – vytvořit a spravovat virtuální počítače Windows s modulu Azure PowerShell"
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
ms.openlocfilehash: ec1bb7834beb66dc28dd5b1db764bd358243292c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-windows-vms-with-the-azure-powershell-module"></a><span data-ttu-id="8405f-103">Vytvoření a správa virtuálních počítačů Windows s modulu Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="8405f-103">Create and Manage Windows VMs with the Azure PowerShell module</span></span>

<span data-ttu-id="8405f-104">Virtuální počítače Azure, zadejte plně konfigurovatelné a flexibilní výpočetního prostředí.</span><span class="sxs-lookup"><span data-stu-id="8405f-104">Azure virtual machines provide a fully configurable and flexible computing environment.</span></span> <span data-ttu-id="8405f-105">Tento kurz se zaměřuje na základní virtuální počítač Azure nasazení položky například vyberete velikost virtuálního počítače, výběr image virtuálního počítače a nasazení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8405f-105">This tutorial covers basic Azure virtual machine deployment items such as selecting a VM size, selecting a VM image, and deploying a VM.</span></span> <span data-ttu-id="8405f-106">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="8405f-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8405f-107">Vytvořit a připojit k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="8405f-107">Create and connect to a VM</span></span>
> * <span data-ttu-id="8405f-108">Vyberte a použijte Image virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="8405f-108">Select and use VM images</span></span>
> * <span data-ttu-id="8405f-109">Zobrazení a používání určité velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="8405f-109">View and use specific VM sizes</span></span>
> * <span data-ttu-id="8405f-110">Změna velikosti virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="8405f-110">Resize a VM</span></span>
> * <span data-ttu-id="8405f-111">Zobrazení a pochopit stav virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="8405f-111">View and understand VM state</span></span>

<span data-ttu-id="8405f-112">Tento kurz vyžaduje modul Azure PowerShell verze 3.6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="8405f-112">This tutorial requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="8405f-113">Verzi zjistíte spuštěním příkazu ` Get-Module -ListAvailable AzureRM`.</span><span class="sxs-lookup"><span data-stu-id="8405f-113">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="8405f-114">Pokud je třeba upgradovat, přečtěte si téma [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="8405f-114">If you need to upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="create-resource-group"></a><span data-ttu-id="8405f-115">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="8405f-115">Create resource group</span></span>

<span data-ttu-id="8405f-116">Vytvořte skupinu prostředků pomocí příkazu [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="8405f-116">Create a resource group with the [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) command.</span></span> 

<span data-ttu-id="8405f-117">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="8405f-117">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="8405f-118">Před virtuálního počítače je třeba vytvořit skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="8405f-118">A resource group must be created before a virtual machine.</span></span> <span data-ttu-id="8405f-119">V tomto příkladu skupinu prostředků s názvem *myResourceGroupVM* je vytvořen v *EastUS* oblast.</span><span class="sxs-lookup"><span data-stu-id="8405f-119">In this example, a resource group named *myResourceGroupVM* is created in the *EastUS* region.</span></span> 

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupVM -Location EastUS
```

<span data-ttu-id="8405f-120">Skupina prostředků je zadána při vytváření nebo úpravách virtuálního počítače, které jsou viditelné v rámci tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="8405f-120">The resource group is specified when creating or modifying a VM, which can be seen throughout this tutorial.</span></span>

## <a name="create-virtual-machine"></a><span data-ttu-id="8405f-121">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="8405f-121">Create virtual machine</span></span>

<span data-ttu-id="8405f-122">Virtuální počítač musí být připojen k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="8405f-122">A virtual machine must be connected to a virtual network.</span></span> <span data-ttu-id="8405f-123">Byste komunikovat s virtuálním počítačem pomocí veřejnou IP adresu prostřednictvím karty síťového rozhraní.</span><span class="sxs-lookup"><span data-stu-id="8405f-123">You communicate with the virtual machine using a public IP address through a network interface card.</span></span>

### <a name="create-virtual-network"></a><span data-ttu-id="8405f-124">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="8405f-124">Create virtual network</span></span>

<span data-ttu-id="8405f-125">Vytvořte podsíť s [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="8405f-125">Create a subnet with [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):</span></span>

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
```

<span data-ttu-id="8405f-126">Vytvoření virtuální sítě s [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="8405f-126">Create a virtual network with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 ` 
  -Subnet $subnetConfig
```
### <a name="create-public-ip-address"></a><span data-ttu-id="8405f-127">Vytvoření veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="8405f-127">Create public IP address</span></span>

<span data-ttu-id="8405f-128">Vytvoření veřejné IP adresy s [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):</span><span class="sxs-lookup"><span data-stu-id="8405f-128">Create a public IP address with [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):</span></span>

```powershell
$pip = New-AzureRmPublicIpAddress ` 
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS ` 
  -AllocationMethod Static `
  -Name myPublicIPAddress
```

### <a name="create-network-interface-card"></a><span data-ttu-id="8405f-129">Vytvoření karty síťového rozhraní</span><span class="sxs-lookup"><span data-stu-id="8405f-129">Create network interface card</span></span>

<span data-ttu-id="8405f-130">Vytvoření síťová karta s [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="8405f-130">Create a network interface card with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):</span></span>

```powershell
$nic = New-AzureRmNetworkInterface `
  -ResourceGroupName myResourceGroupVM  `
  -Location EastUS `
  -Name myNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

### <a name="create-network-security-group"></a><span data-ttu-id="8405f-131">Vytvořit skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="8405f-131">Create network security group</span></span>

<span data-ttu-id="8405f-132">Azure [skupinu zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md) (NSG) řídí příchozí a odchozí přenosy pro jeden nebo více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8405f-132">An Azure [network security group](../../virtual-network/virtual-networks-nsg.md) (NSG) controls inbound and outbound traffic for one or many virtual machines.</span></span> <span data-ttu-id="8405f-133">Pravidla skupiny zabezpečení sítě, povolit nebo odepřít síťový provoz na konkrétní port nebo rozsah portů.</span><span class="sxs-lookup"><span data-stu-id="8405f-133">Network security group rules allow or deny network traffic on a specific port or port range.</span></span> <span data-ttu-id="8405f-134">Tato pravidla může zahrnovat předpona zdrojové adresy tak, aby jenom přenosy v předdefinované zdroj může komunikovat s virtuálním počítačem.</span><span class="sxs-lookup"><span data-stu-id="8405f-134">These rules can also include a source address prefix so that only traffic originating at a predefined source can communicate with a virtual machine.</span></span> <span data-ttu-id="8405f-135">Pro přístup k webovém serveru IIS, který se instaluje, je nutné přidat příchozího pravidla NSG.</span><span class="sxs-lookup"><span data-stu-id="8405f-135">To access the IIS webserver that you are installing, you must add an inbound NSG rule.</span></span>

<span data-ttu-id="8405f-136">Vytvoření příchozího pravidla NSG, použijte [přidat AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig).</span><span class="sxs-lookup"><span data-stu-id="8405f-136">To create an inbound NSG rule, use [Add-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig).</span></span> <span data-ttu-id="8405f-137">Následující příklad vytvoří pravidlo NSG s názvem *myNSGRule* , otevře port *3389* pro virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="8405f-137">The following example creates an NSG rule named *myNSGRule* that opens port *3389* for the virtual machine:</span></span>

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

<span data-ttu-id="8405f-138">Vytvořte pomocí NSG *myNSGRule* s [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):</span><span class="sxs-lookup"><span data-stu-id="8405f-138">Create the NSG using *myNSGRule* with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):</span></span>

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupVM `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRule
```

<span data-ttu-id="8405f-139">Přidat NSG k podsíti v dané virtuální síti s [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="8405f-139">Add the NSG to the subnet in the virtual network with [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):</span></span>

```powershell
Set-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -VirtualNetwork $vnet `
    -NetworkSecurityGroup $nsg `
    -AddressPrefix 192.168.1.0/24
```

<span data-ttu-id="8405f-140">Aktualizace virtuální sítě s [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="8405f-140">Update the virtual network with [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):</span></span>

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-machine"></a><span data-ttu-id="8405f-141">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="8405f-141">Create virtual machine</span></span>

<span data-ttu-id="8405f-142">Při vytváření virtuálního počítače, jsou k dispozici jako jsou bitové kopie operačního systému, přihlašovací údaje velikost a správu disku několik možností.</span><span class="sxs-lookup"><span data-stu-id="8405f-142">When creating a virtual machine, several options are available such as operating system image, disk sizing, and administrative credentials.</span></span> <span data-ttu-id="8405f-143">V tomto příkladu se vytvoří virtuální počítač s názvem *Můjvp* s nejnovější verzí systému Windows Server 2016 Datacenter.</span><span class="sxs-lookup"><span data-stu-id="8405f-143">In this example, a virtual machine is created with a name of *myVM* running the latest version of Windows Server 2016 Datacenter.</span></span>

<span data-ttu-id="8405f-144">Nastavte uživatelské jméno a heslo, které potřebuje pro účet správce na virtuálním počítači s [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="8405f-144">Set the username and password needed for the administrator account on the virtual machine with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="8405f-145">Vytvoření počáteční konfigurace pro virtuální počítač s [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig):</span><span class="sxs-lookup"><span data-stu-id="8405f-145">Create the initial configuration for the virtual machine with [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig):</span></span>

```powershell
$vm = New-AzureRmVMConfig -VMName myVM -VMSize Standard_D1
```

<span data-ttu-id="8405f-146">Přidat informace operačního systému do konfigurace virtuálního počítače s [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem):</span><span class="sxs-lookup"><span data-stu-id="8405f-146">Add the operating system information to the virtual machine configuration with [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem):</span></span>

```powershell
$vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM `
    -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
```

<span data-ttu-id="8405f-147">Přidat informace o obrázku v konfiguraci virtuálního počítače s [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage):</span><span class="sxs-lookup"><span data-stu-id="8405f-147">Add the image information to the virtual machine configuration with [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage):</span></span>

```powershell
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
```

<span data-ttu-id="8405f-148">Přidejte nastavení disku operačního systému do konfigurace virtuálního počítače s [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk):</span><span class="sxs-lookup"><span data-stu-id="8405f-148">Add the operating system disk settings to the virtual machine configuration with [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk):</span></span>

```powershell
$vm = Set-AzureRmVMOSDisk `
    -VM $vm `
    -Name myOsDisk `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
```

<span data-ttu-id="8405f-149">Přidat síťový adaptér, který jste předtím vytvořili pro konfiguraci virtuálního počítače s [přidat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="8405f-149">Add the network interface card that you previously created to the virtual machine configuration with [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span></span>

```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

<span data-ttu-id="8405f-150">Vytvořte virtuální počítač pomocí příkazu [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="8405f-150">Create the virtual machine with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span>

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroupVM -Location EastUS -VM $vm
```

## <a name="connect-to-vm"></a><span data-ttu-id="8405f-151">Připojit k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="8405f-151">Connect to VM</span></span>

<span data-ttu-id="8405f-152">Po dokončení nasazení vytvořte připojení ke vzdálené ploše virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8405f-152">After the deployment has completed, create a remote desktop connection with the virtual machine.</span></span>

<span data-ttu-id="8405f-153">Spuštění následujících příkazů vrátí veřejnou IP adresu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8405f-153">Run the following commands to return the public IP address of the virtual machine.</span></span> <span data-ttu-id="8405f-154">Poznamenejte si tuto IP adresu, abyste se k ní v dalším kroku mohli pomocí prohlížeče připojit a otestovat připojení k webu.</span><span class="sxs-lookup"><span data-stu-id="8405f-154">Take note of this IP Address so you can connect to it with your browser to test web connectivity in a future step.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroupVM  | Select IpAddress
```

<span data-ttu-id="8405f-155">Pomocí následujícího příkazu vytvořte s virtuálním počítačem relaci vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="8405f-155">Use the following command to create a remote desktop session with the virtual machine.</span></span> <span data-ttu-id="8405f-156">IP adresu nahraďte veřejnou IP adresou (*publicIPAddress*) vašeho virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8405f-156">Replace the IP address with the *publicIPAddress* of your virtual machine.</span></span> <span data-ttu-id="8405f-157">Po zobrazení výzvy zadejte přihlašovací údaje, které jste použili při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8405f-157">When prompted, enter the credentials used when creating the virtual machine.</span></span>

```powershell
mstsc /v:<publicIpAddress>
```

## <a name="understand-vm-images"></a><span data-ttu-id="8405f-158">Pochopení Image virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="8405f-158">Understand VM images</span></span>

<span data-ttu-id="8405f-159">Azure marketplace obsahuje mnoho bitové kopie virtuálních počítačů, které lze použít k vytvoření nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8405f-159">The Azure marketplace includes many virtual machine images that can be used to create a new virtual machine.</span></span> <span data-ttu-id="8405f-160">V předchozích krocích byl vytvořen virtuální počítač pomocí bitové kopie systému Windows Server 2016-Datacenter.</span><span class="sxs-lookup"><span data-stu-id="8405f-160">In the previous steps, a virtual machine was created using the Windows Server 2016-Datacenter image.</span></span> <span data-ttu-id="8405f-161">V tomto kroku modulu PowerShell se používá pro vyhledávání na webu marketplace pro jiné Image systému Windows, které může taky jako základ pro nové virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="8405f-161">In this step, the PowerShell module is used to search the marketplace for other Windows images, which can also as a base for new VMs.</span></span> <span data-ttu-id="8405f-162">Tento proces se skládá z hledání vydavatele, nabídky a název bitové kopie (Sku).</span><span class="sxs-lookup"><span data-stu-id="8405f-162">This process consists of finding the publisher, offer, and the image name (Sku).</span></span> 

<span data-ttu-id="8405f-163">Použití [Get-AzureRmVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher) příkaz vrátí seznam vydavatelů bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="8405f-163">Use the [Get-AzureRmVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher) command to return a list of image publishers.</span></span>  

```powersehll
Get-AzureRmVMImagePublisher -Location "EastUS"
```

<span data-ttu-id="8405f-164">Použití [Get-AzureRmVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) vrácení seznamu nabízí bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="8405f-164">Use the [Get-AzureRmVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) to return a list of image offers.</span></span> <span data-ttu-id="8405f-165">Pomocí tohoto příkazu vrácený seznam je filtrován na zadaný vydavatel.</span><span class="sxs-lookup"><span data-stu-id="8405f-165">With this command, the returned list is filtered on the specified publisher.</span></span> 

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

<span data-ttu-id="8405f-166">[Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) příkazu vyfiltruje klikněte na název vydavatele a nabídky k zobrazení seznamu názvů bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="8405f-166">The [Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) command will then filter on the publisher and offer name to return a list of image names.</span></span>

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

<span data-ttu-id="8405f-167">Tyto informace slouží k nasazení virtuálních počítačů s konkrétní image.</span><span class="sxs-lookup"><span data-stu-id="8405f-167">This information can be used to deploy a VM with a specific image.</span></span> <span data-ttu-id="8405f-168">Tento příklad nastaví název bitové kopie na objekt virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8405f-168">This example sets the image name on the VM object.</span></span> <span data-ttu-id="8405f-169">Naleznete v předchozích příkladech v tomto kurzu postup dokončení nasazení.</span><span class="sxs-lookup"><span data-stu-id="8405f-169">Refer to the previous examples in this tutorial for complete deployment steps.</span></span>

```powershell
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter-with-Containers `
    -Version latest
```

## <a name="understand-vm-sizes"></a><span data-ttu-id="8405f-170">Pochopení velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="8405f-170">Understand VM sizes</span></span>

<span data-ttu-id="8405f-171">Velikost virtuálního počítače určuje množství výpočetní prostředky, jako je například procesoru, grafického procesoru a paměti, které jsou k dispozici pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="8405f-171">A virtual machine size determines the amount of compute resources such as CPU, GPU, and memory that are made available to the virtual machine.</span></span> <span data-ttu-id="8405f-172">Virtuální počítače je potřeba vytvořit s velikostí odpovídající expect pracovní zátěže.</span><span class="sxs-lookup"><span data-stu-id="8405f-172">Virtual machines need to be created with a size appropriate for the expect work load.</span></span> <span data-ttu-id="8405f-173">Hodnota se zvyšuje zatížení, můžete ke změně velikosti existujícího virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8405f-173">If workload increases, an existing virtual machine can be resized.</span></span>

### <a name="vm-sizes"></a><span data-ttu-id="8405f-174">Velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="8405f-174">VM Sizes</span></span>

<span data-ttu-id="8405f-175">V následující tabulce velikostí rozděluje do případy použití.</span><span class="sxs-lookup"><span data-stu-id="8405f-175">The following table categorizes sizes into use cases.</span></span>  

| <span data-ttu-id="8405f-176">Typ</span><span class="sxs-lookup"><span data-stu-id="8405f-176">Type</span></span>                     | <span data-ttu-id="8405f-177">Velikosti</span><span class="sxs-lookup"><span data-stu-id="8405f-177">Sizes</span></span>           |    <span data-ttu-id="8405f-178">Popis</span><span class="sxs-lookup"><span data-stu-id="8405f-178">Description</span></span>       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8405f-179">Obecné účely</span><span class="sxs-lookup"><span data-stu-id="8405f-179">General purpose</span></span>         |<span data-ttu-id="8405f-180">DSv2, Dv2, DS, D, Av2, A0 7</span><span class="sxs-lookup"><span data-stu-id="8405f-180">DSv2, Dv2, DS, D, Av2, A0-7</span></span>| <span data-ttu-id="8405f-181">Vyrovnáváním procesoru do paměti.</span><span class="sxs-lookup"><span data-stu-id="8405f-181">Balanced CPU-to-memory.</span></span> <span data-ttu-id="8405f-182">Ideální pro vývoj / testování a malé a střední řešení aplikacím a datům.</span><span class="sxs-lookup"><span data-stu-id="8405f-182">Ideal for dev / test and small to medium applications and data solutions.</span></span>  |
| <span data-ttu-id="8405f-183">Optimalizované z hlediska výpočetních služeb</span><span class="sxs-lookup"><span data-stu-id="8405f-183">Compute optimized</span></span>      | <span data-ttu-id="8405f-184">Služby FS, F</span><span class="sxs-lookup"><span data-stu-id="8405f-184">Fs, F</span></span>             | <span data-ttu-id="8405f-185">Vysoké využití procesoru do paměti.</span><span class="sxs-lookup"><span data-stu-id="8405f-185">High CPU-to-memory.</span></span> <span data-ttu-id="8405f-186">Je vhodný pro střední provoz aplikace, síťových zařízení a dávkových procesů.</span><span class="sxs-lookup"><span data-stu-id="8405f-186">Good for medium traffic applications, network appliances, and batch processes.</span></span>        |
| <span data-ttu-id="8405f-187">Optimalizované z hlediska paměti</span><span class="sxs-lookup"><span data-stu-id="8405f-187">Memory optimized</span></span>       | <span data-ttu-id="8405f-188">GS, G, DSv2, DS, Dv2, D</span><span class="sxs-lookup"><span data-stu-id="8405f-188">GS, G, DSv2, DS, Dv2, D</span></span>   | <span data-ttu-id="8405f-189">Vysoká paměti na core.</span><span class="sxs-lookup"><span data-stu-id="8405f-189">High memory-to-core.</span></span> <span data-ttu-id="8405f-190">Výborně hodí pro relačních databází, středních a velkých mezipaměti a analýzy v paměti.</span><span class="sxs-lookup"><span data-stu-id="8405f-190">Great for relational databases, medium to large caches, and in-memory analytics.</span></span>                 |
| <span data-ttu-id="8405f-191">Optimalizované z hlediska úložiště</span><span class="sxs-lookup"><span data-stu-id="8405f-191">Storage optimized</span></span>       | <span data-ttu-id="8405f-192">Ls</span><span class="sxs-lookup"><span data-stu-id="8405f-192">Ls</span></span>                | <span data-ttu-id="8405f-193">Vysoká propustnost disku a V/V.</span><span class="sxs-lookup"><span data-stu-id="8405f-193">High disk throughput and IO.</span></span> <span data-ttu-id="8405f-194">Ideální pro databáze NoSQL, SQL a velké objemy dat.</span><span class="sxs-lookup"><span data-stu-id="8405f-194">Ideal for Big Data, SQL, and NoSQL databases.</span></span>                                                         |
| <span data-ttu-id="8405f-195">GPU</span><span class="sxs-lookup"><span data-stu-id="8405f-195">GPU</span></span>           | <span data-ttu-id="8405f-196">VS, NC</span><span class="sxs-lookup"><span data-stu-id="8405f-196">NV, NC</span></span>            | <span data-ttu-id="8405f-197">Specializované virtuální počítače cílené pro velkou grafické vykreslování a úpravy videa.</span><span class="sxs-lookup"><span data-stu-id="8405f-197">Specialized VMs targeted for heavy graphic rendering and video editing.</span></span>       |
| <span data-ttu-id="8405f-198">Vysoký výkon</span><span class="sxs-lookup"><span data-stu-id="8405f-198">High performance</span></span> | <span data-ttu-id="8405f-199">H, A8 11</span><span class="sxs-lookup"><span data-stu-id="8405f-199">H, A8-11</span></span>          | <span data-ttu-id="8405f-200">Naše nejúčinnějších procesoru virtuálních počítačů s volitelné vysokou propustností síťová rozhraní (počítače RDMA).</span><span class="sxs-lookup"><span data-stu-id="8405f-200">Our most powerful CPU VMs with optional high-throughput network interfaces (RDMA).</span></span> 


### <a name="find-available-vm-sizes"></a><span data-ttu-id="8405f-201">Najít dostupných velikostí virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="8405f-201">Find available VM sizes</span></span>

<span data-ttu-id="8405f-202">Pokud chcete zobrazit seznam velikostí virtuálních počítačů v určité oblasti, použijte [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) příkaz.</span><span class="sxs-lookup"><span data-stu-id="8405f-202">To see a list of VM sizes available in a particular region, use the [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) command.</span></span>

```powershell
Get-AzureRmVMSize -Location EastUS
```

## <a name="resize-a-vm"></a><span data-ttu-id="8405f-203">Změna velikosti virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="8405f-203">Resize a VM</span></span>

<span data-ttu-id="8405f-204">Po nasazení virtuálního počítače, můžete změnit velikost můžete zvýšit nebo snížit přidělení prostředků.</span><span class="sxs-lookup"><span data-stu-id="8405f-204">After a VM has been deployed, it can be resized to increase or decrease resource allocation.</span></span>

<span data-ttu-id="8405f-205">Před změnou velikosti virtuálního počítače, zkontrolujte, jestli požadovaná velikost je dostupná na aktuální cluster virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8405f-205">Before resizing a VM, check if the desired size is available on the current VM cluster.</span></span> <span data-ttu-id="8405f-206">[Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) příkaz vrátí seznam velikostí.</span><span class="sxs-lookup"><span data-stu-id="8405f-206">The [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) command returns a list of sizes.</span></span> 

```powershell
Get-AzureRmVMSize -ResourceGroupName myResourceGroupVM -VMName myVM 
```

<span data-ttu-id="8405f-207">Pokud požadovaná velikost je k dispozici, virtuální počítač velikost lze změnit ze stavu na zapnuté, ale po restartu během operace.</span><span class="sxs-lookup"><span data-stu-id="8405f-207">If the desired size is available, the VM can be resized from a powered-on state, however it is rebooted during the operation.</span></span>

```powershell
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM 
$vm.HardwareProfile.VmSize = "Standard_D4"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
```

<span data-ttu-id="8405f-208">Pokud požadovaná velikost není v aktuální clusteru, musí být navrácena, než dojde k operace změny velikosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8405f-208">If the desired size is not on the current cluster, the VM needs to be deallocated before the resize operation can occur.</span></span> <span data-ttu-id="8405f-209">Všimněte si, když virtuální počítač je zapnutý zpět, se odeberou všechna data na dočasné disku a veřejné IP adresa změnu, pokud se používá statickou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="8405f-209">Note, when the VM is powered back on, any data on the temp disk are removed, and the public IP address change unless a static IP address is being used.</span></span> 

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM
$vm.HardwareProfile.VmSize = "Standard_F4s"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
Start-AzureRmVM -ResourceGroupName myResourceGroupVM  -Name $vm.name
```

## <a name="vm-power-states"></a><span data-ttu-id="8405f-210">Stavy spotřeby virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="8405f-210">VM power states</span></span>

<span data-ttu-id="8405f-211">Virtuální počítač Azure může mít jednu z mnoha snížené spotřeby energie.</span><span class="sxs-lookup"><span data-stu-id="8405f-211">An Azure VM can have one of many power states.</span></span> <span data-ttu-id="8405f-212">Tento stav představuje aktuální stav virtuálního počítače z hlediska hypervisoru.</span><span class="sxs-lookup"><span data-stu-id="8405f-212">This state represents the current state of the VM from the standpoint of the hypervisor.</span></span> 

### <a name="power-states"></a><span data-ttu-id="8405f-213">Stavy spotřeby.</span><span class="sxs-lookup"><span data-stu-id="8405f-213">Power states</span></span>

| <span data-ttu-id="8405f-214">Stav napájení</span><span class="sxs-lookup"><span data-stu-id="8405f-214">Power State</span></span> | <span data-ttu-id="8405f-215">Popis</span><span class="sxs-lookup"><span data-stu-id="8405f-215">Description</span></span>
|----|----|
| <span data-ttu-id="8405f-216">Spouštění</span><span class="sxs-lookup"><span data-stu-id="8405f-216">Starting</span></span> | <span data-ttu-id="8405f-217">Označuje, že virtuální počítač se spouští.</span><span class="sxs-lookup"><span data-stu-id="8405f-217">Indicates the virtual machine is being started.</span></span> |
| <span data-ttu-id="8405f-218">Běžící (Spuštěno)</span><span class="sxs-lookup"><span data-stu-id="8405f-218">Running</span></span> | <span data-ttu-id="8405f-219">Určuje, zda je virtuální počítač spuštěn.</span><span class="sxs-lookup"><span data-stu-id="8405f-219">Indicates that the virtual machine is running.</span></span> |
| <span data-ttu-id="8405f-220">Zastavování</span><span class="sxs-lookup"><span data-stu-id="8405f-220">Stopping</span></span> | <span data-ttu-id="8405f-221">Označuje, že je zastavení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8405f-221">Indicates that the virtual machine is being stopped.</span></span> | 
| <span data-ttu-id="8405f-222">Zastaveno</span><span class="sxs-lookup"><span data-stu-id="8405f-222">Stopped</span></span> | <span data-ttu-id="8405f-223">Označuje, že virtuální počítač je zastavená.</span><span class="sxs-lookup"><span data-stu-id="8405f-223">Indicates that the virtual machine is stopped.</span></span> <span data-ttu-id="8405f-224">Všimněte si, že virtuální počítače v zastaveném stavu stále platit poplatky výpočty.</span><span class="sxs-lookup"><span data-stu-id="8405f-224">Note that virtual machines in the stopped state still incur compute charges.</span></span>  |
| <span data-ttu-id="8405f-225">Rušení přidělování</span><span class="sxs-lookup"><span data-stu-id="8405f-225">Deallocating</span></span> | <span data-ttu-id="8405f-226">Označuje, že virtuální počítač je navrácena.</span><span class="sxs-lookup"><span data-stu-id="8405f-226">Indicates that the virtual machine is being deallocated.</span></span> |
| <span data-ttu-id="8405f-227">Zrušeno</span><span class="sxs-lookup"><span data-stu-id="8405f-227">Deallocated</span></span> | <span data-ttu-id="8405f-228">Označuje, že virtuální počítač je úplně odebrána z hypervisor, ale stále k dispozici v rovině řízení.</span><span class="sxs-lookup"><span data-stu-id="8405f-228">Indicates that the virtual machine is completely removed from the hypervisor but still available in the control plane.</span></span> <span data-ttu-id="8405f-229">Virtuální počítače ve stavu Deallocated nevznikají poplatky za výpočty.</span><span class="sxs-lookup"><span data-stu-id="8405f-229">Virtual machines in the Deallocated state do not incur compute charges.</span></span> |
| - | <span data-ttu-id="8405f-230">Označuje, že stav napájení virtuálního počítače neznámý.</span><span class="sxs-lookup"><span data-stu-id="8405f-230">Indicates that the power state of the virtual machine is unknown.</span></span> |

### <a name="find-power-state"></a><span data-ttu-id="8405f-231">Najít stav napájení</span><span class="sxs-lookup"><span data-stu-id="8405f-231">Find power state</span></span>

<span data-ttu-id="8405f-232">Chcete-li načíst stav konkrétní virtuální počítač, použijte [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) příkaz.</span><span class="sxs-lookup"><span data-stu-id="8405f-232">To retrieve the state of a particular VM, use the [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) command.</span></span> <span data-ttu-id="8405f-233">Je nutné zadat platný název pro virtuální počítač a skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="8405f-233">Be sure to specify a valid name for a virtual machine and resource group.</span></span> 

```powershell
Get-AzureRmVM `
    -ResourceGroupName myResourceGroup `
    -Name myVM `
    -Status | Select @{n="Status"; e={$_.Statuses[1].Code}}
```

<span data-ttu-id="8405f-234">Výstup:</span><span class="sxs-lookup"><span data-stu-id="8405f-234">Output:</span></span>

```powershell
Status
------
PowerState/running
```

## <a name="management-tasks"></a><span data-ttu-id="8405f-235">Úlohy správy</span><span class="sxs-lookup"><span data-stu-id="8405f-235">Management tasks</span></span>

<span data-ttu-id="8405f-236">Během životního cyklu virtuálního počítače můžete spustit úlohy správy, jako je například spuštění, zastavení nebo odstranění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8405f-236">During the lifecycle of a virtual machine, you may want to run management tasks such as starting, stopping, or deleting a virtual machine.</span></span> <span data-ttu-id="8405f-237">Kromě toho můžete vytvořit skripty pro automatizaci úloh opakovaných nebo komplexní.</span><span class="sxs-lookup"><span data-stu-id="8405f-237">Additionally, you may want to create scripts to automate repetitive or complex tasks.</span></span> <span data-ttu-id="8405f-238">Pomocí Azure PowerShell, mnoho běžné úlohy správy lze spustit z příkazového řádku nebo ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="8405f-238">Using Azure PowerShell, many common management tasks can be run from the command line or in scripts.</span></span>

### <a name="stop-virtual-machine"></a><span data-ttu-id="8405f-239">Zastavit virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="8405f-239">Stop virtual machine</span></span>

<span data-ttu-id="8405f-240">Zastavení a zrušit přidělení virtuálního počítače s [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="8405f-240">Stop and deallocate a virtual machine with [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm):</span></span>

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
```

<span data-ttu-id="8405f-241">Pokud chcete zachovat virtuální počítač ve stavu, zřízený, použijte parametr - StayProvisioned.</span><span class="sxs-lookup"><span data-stu-id="8405f-241">If you want to keep the virtual machine in a provisioned state, use the -StayProvisioned parameter.</span></span>

### <a name="start-virtual-machine"></a><span data-ttu-id="8405f-242">Spustit virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="8405f-242">Start virtual machine</span></span>

```powershell
Start-AzureRmVM -ResourceGroupName myResourceGroupVM -Name myVM
```

### <a name="delete-resource-group"></a><span data-ttu-id="8405f-243">Odstraňte skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="8405f-243">Delete resource group</span></span>

<span data-ttu-id="8405f-244">Odstranění skupiny prostředků se také odstraní všechny prostředky obsažené v rámci.</span><span class="sxs-lookup"><span data-stu-id="8405f-244">Deleting a resource group also deletes all resources contained within.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroupVM -Force
```

## <a name="next-steps"></a><span data-ttu-id="8405f-245">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8405f-245">Next steps</span></span>

<span data-ttu-id="8405f-246">V tomto kurzu jste se dozvěděli o základní vytvoření virtuálního počítače a správy, jako například:</span><span class="sxs-lookup"><span data-stu-id="8405f-246">In this tutorial, you learned about basic VM creation and management such as how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8405f-247">Vytvořit a připojit k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="8405f-247">Create and connect to a VM</span></span>
> * <span data-ttu-id="8405f-248">Vyberte a použijte Image virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="8405f-248">Select and use VM images</span></span>
> * <span data-ttu-id="8405f-249">Zobrazení a používání určité velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="8405f-249">View and use specific VM sizes</span></span>
> * <span data-ttu-id="8405f-250">Změna velikosti virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="8405f-250">Resize a VM</span></span>
> * <span data-ttu-id="8405f-251">Zobrazení a pochopit stav virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="8405f-251">View and understand VM state</span></span>

<span data-ttu-id="8405f-252">Přechodu na v dalším kurzu se dozvíte o disky virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8405f-252">Advance to the next tutorial to learn about VM disks.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="8405f-253">Vytvoření a správa virtuálních počítačů disky</span><span class="sxs-lookup"><span data-stu-id="8405f-253">Create and Manage VM disks</span></span>](./tutorial-manage-data-disk.md)
