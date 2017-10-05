---
title: "Přizpůsobení systému Windows virtuálního počítače v Azure | Microsoft Docs"
description: "Další informace o použití rozšíření vlastních skriptů a Key Vault pro přizpůsobení systému Windows virtuálních počítačů v Azure"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 3be58bf8afbcff018b2b0d69a0e08c2c9ab1fca7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-customize-a-windows-virtual-machine-in-azure"></a><span data-ttu-id="bc961-103">Přizpůsobení virtuálního počítače s Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="bc961-103">How to customize a Windows virtual machine in Azure</span></span>
<span data-ttu-id="bc961-104">Ke konfiguraci virtuálních počítačů (VM) rychlé a konzistentním způsobem, je obvykle potřeby nějaký způsob automatizace.</span><span class="sxs-lookup"><span data-stu-id="bc961-104">To configure virtual machines (VMs) in a quick and consistent manner, some form of automation is typically desired.</span></span> <span data-ttu-id="bc961-105">Běžný postup přizpůsobení virtuální počítač s Windows je použití [vlastní skript rozšíření pro Windows](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="bc961-105">A common approach to customize a Windows VM is to use [Custom Script Extension for Windows](extensions-customscript.md).</span></span> <span data-ttu-id="bc961-106">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="bc961-106">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bc961-107">Použití rozšíření vlastních skriptů instalace služby IIS</span><span class="sxs-lookup"><span data-stu-id="bc961-107">Use the Custom Script Extension to install IIS</span></span>
> * <span data-ttu-id="bc961-108">Vytvoření virtuálního počítače, který používá rozšíření vlastních skriptů</span><span class="sxs-lookup"><span data-stu-id="bc961-108">Create a VM that uses the Custom Script Extension</span></span>
> * <span data-ttu-id="bc961-109">Po rozšíření platí zobrazit spuštěné webu IIS</span><span class="sxs-lookup"><span data-stu-id="bc961-109">View a running IIS site after the extension is applied</span></span>

<span data-ttu-id="bc961-110">Tento kurz vyžaduje modul Azure PowerShell verze 3.6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="bc961-110">This tutorial requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="bc961-111">Verzi zjistíte spuštěním příkazu ` Get-Module -ListAvailable AzureRM`.</span><span class="sxs-lookup"><span data-stu-id="bc961-111">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="bc961-112">Pokud je třeba upgradovat, přečtěte si téma [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="bc961-112">If you need to upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>


## <a name="custom-script-extension-overview"></a><span data-ttu-id="bc961-113">Přehled rozšíření vlastních skriptů</span><span class="sxs-lookup"><span data-stu-id="bc961-113">Custom script extension overview</span></span>
<span data-ttu-id="bc961-114">Rozšíření vlastních skriptů stahuje a spouští skripty na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="bc961-114">The Custom Script Extension downloads and executes scripts on Azure VMs.</span></span> <span data-ttu-id="bc961-115">Toto rozšíření je užitečné pro konfiguraci nasazení post, instalace softwaru nebo jakoukoli jinou konfiguraci, nebo úlohu správy.</span><span class="sxs-lookup"><span data-stu-id="bc961-115">This extension is useful for post deployment configuration, software installation, or any other configuration / management task.</span></span> <span data-ttu-id="bc961-116">Skripty můžete stáhnout z úložiště Azure nebo GitHub nebo zadat na portál Azure na dobu běhu rozšíření.</span><span class="sxs-lookup"><span data-stu-id="bc961-116">Scripts can be downloaded from Azure storage or GitHub, or provided to the Azure portal at extension run time.</span></span>

<span data-ttu-id="bc961-117">Rozšíření vlastních skriptů se integruje s šablon Azure Resource Manageru a můžete také spustit pomocí rozhraní příkazového řádku Azure, PowerShell, portálu Azure nebo REST API pro virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="bc961-117">The Custom Script extension integrates with Azure Resource Manager templates, and can also be run using the Azure CLI, PowerShell, Azure portal, or the Azure Virtual Machine REST API.</span></span>

<span data-ttu-id="bc961-118">S Windows a virtuální počítače s Linuxem můžete použít rozšíření vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="bc961-118">You can use the Custom Script Extension with both Windows and Linux VMs.</span></span>


## <a name="create-virtual-machine"></a><span data-ttu-id="bc961-119">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="bc961-119">Create virtual machine</span></span>
<span data-ttu-id="bc961-120">Před vytvořením virtuálního počítače, vytvořte skupinu prostředků s [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="bc961-120">Before you can create a VM, create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="bc961-121">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupAutomate* v *EastUS* umístění:</span><span class="sxs-lookup"><span data-stu-id="bc961-121">The following example creates a resource group named *myResourceGroupAutomate* in the *EastUS* location:</span></span>

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupAutomate -Location EastUS
```

<span data-ttu-id="bc961-122">Nastavte správce uživatelské jméno a heslo pro virtuální počítače s [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="bc961-122">Set an administrator username and password for the VMs with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="bc961-123">Nyní můžete vytvořit virtuální počítač s [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="bc961-123">Now you can create the VM with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="bc961-124">Následující příklad vytvoří virtuální sítě požadované součásti, konfigurace operačního systému a poté vytvoří virtuální počítač s názvem *Můjvp*:</span><span class="sxs-lookup"><span data-stu-id="bc961-124">The following example creates the required virtual network components, the OS configuration, and then creates a VM named *myVM*:</span></span>

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -Name myVnet `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$publicIP = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4 `
    -Name "myPublicIP"

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleRDP  `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleWWW  `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1001 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 80 `
    -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP,$nsgRuleWeb

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
    -Name myNic `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIP.Id `
    -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS2 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer -Skus 2016-Datacenter -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

New-AzureRmVM -ResourceGroupName myResourceGroupAutomate -Location EastUS -VM $vmConfig
```

<span data-ttu-id="bc961-125">Trvá několik minut na zdroje a virtuální počítač, který se má vytvořit.</span><span class="sxs-lookup"><span data-stu-id="bc961-125">It takes a few minutes for the resources and VM to be created.</span></span>


## <a name="automate-iis-install"></a><span data-ttu-id="bc961-126">Automatizaci instalace služby IIS</span><span class="sxs-lookup"><span data-stu-id="bc961-126">Automate IIS install</span></span>
<span data-ttu-id="bc961-127">Použití [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) k instalaci rozšíření vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="bc961-127">Use [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) to install the Custom Script Extension.</span></span> <span data-ttu-id="bc961-128">Spustí rozšíření `powershell Add-WindowsFeature Web-Server` nainstalovat webový server služby IIS a aktualizací *Default.htm* stránku a zobrazit název hostitele virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="bc961-128">The extension runs `powershell Add-WindowsFeature Web-Server` to install the IIS webserver and then updates the *Default.htm* page to show the hostname of the VM:</span></span>

```powershell
Set-AzureRmVMExtension -ResourceGroupName myResourceGroupAutomate `
    -ExtensionName IIS `
    -VMName myVM `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
    -Location EastUS
```


## <a name="test-web-site"></a><span data-ttu-id="bc961-129">Test webu</span><span class="sxs-lookup"><span data-stu-id="bc961-129">Test web site</span></span>
<span data-ttu-id="bc961-130">Získat veřejnou IP adresu nástroj pro vyrovnávání zatížení s [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="bc961-130">Obtain the public IP address of your load balancer with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="bc961-131">Následující příklad, získá IP adresu pro *myPublicIP* vytvořili dříve:</span><span class="sxs-lookup"><span data-stu-id="bc961-131">The following example obtains the IP address for *myPublicIP* created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Name myPublicIP | select IpAddress
```

<span data-ttu-id="bc961-132">Potom můžete zadat veřejnou IP adresu v do webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="bc961-132">You can then enter the public IP address in to a web browser.</span></span> <span data-ttu-id="bc961-133">Zobrazí se na webu, včetně názvu hostitele virtuálního počítače, který nástroje pro vyrovnávání zatížení distribuován provoz jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="bc961-133">The website is displayed, including the hostname of the VM that the load balancer distributed traffic to as in the following example:</span></span>

![Spuštění webu IIS](./media/tutorial-automate-vm-deployment/running-iis-website.png)


## <a name="next-steps"></a><span data-ttu-id="bc961-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bc961-135">Next steps</span></span>

<span data-ttu-id="bc961-136">V tomto kurzu automatizované instalace služby IIS na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="bc961-136">In this tutorial, you automated the IIS install on a VM.</span></span> <span data-ttu-id="bc961-137">Jste se dozvěděli, jak na:</span><span class="sxs-lookup"><span data-stu-id="bc961-137">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bc961-138">Použití rozšíření vlastních skriptů instalace služby IIS</span><span class="sxs-lookup"><span data-stu-id="bc961-138">Use the Custom Script Extension to install IIS</span></span>
> * <span data-ttu-id="bc961-139">Vytvoření virtuálního počítače, který používá rozšíření vlastních skriptů</span><span class="sxs-lookup"><span data-stu-id="bc961-139">Create a VM that uses the Custom Script Extension</span></span>
> * <span data-ttu-id="bc961-140">Po rozšíření platí zobrazit spuštěné webu IIS</span><span class="sxs-lookup"><span data-stu-id="bc961-140">View a running IIS site after the extension is applied</span></span>

<span data-ttu-id="bc961-141">Přechodu na v dalším kurzu se dozvíte, jak vytvořit vlastní Image virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="bc961-141">Advance to the next tutorial to learn how to create custom VM images.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="bc961-142">Vytváření vlastních imagí virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="bc961-142">Create custom VM images</span></span>](./tutorial-custom-images.md)
