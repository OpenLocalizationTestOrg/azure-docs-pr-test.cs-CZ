---
title: "aaaCustomize virtuální počítač s Windows v Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello rozšíření vlastních skriptů a toocustomize Key Vault Windows virtuálních počítačů v Azure"
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
ms.openlocfilehash: c03b2bb6d70875134c63ea2fe4c2e2c1777c2188
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocustomize-a-windows-virtual-machine-in-azure"></a><span data-ttu-id="f46e3-103">Jak toocustomize virtuálního počítače s Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="f46e3-103">How toocustomize a Windows virtual machine in Azure</span></span>
<span data-ttu-id="f46e3-104">Obvykle se požaduje tooconfigure virtuální počítače (VM) rychlé a konzistentním způsobem, nějaký způsob automatizace.</span><span class="sxs-lookup"><span data-stu-id="f46e3-104">tooconfigure virtual machines (VMs) in a quick and consistent manner, some form of automation is typically desired.</span></span> <span data-ttu-id="f46e3-105">Běžné toocustomize přístup virtuální počítač s Windows je toouse [vlastní skript rozšíření pro Windows](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="f46e3-105">A common approach toocustomize a Windows VM is toouse [Custom Script Extension for Windows](extensions-customscript.md).</span></span> <span data-ttu-id="f46e3-106">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="f46e3-106">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f46e3-107">Použít tooinstall hello rozšíření vlastních skriptů služby IIS</span><span class="sxs-lookup"><span data-stu-id="f46e3-107">Use hello Custom Script Extension tooinstall IIS</span></span>
> * <span data-ttu-id="f46e3-108">Vytvoření virtuálního počítače, který používá hello rozšíření vlastních skriptů</span><span class="sxs-lookup"><span data-stu-id="f46e3-108">Create a VM that uses hello Custom Script Extension</span></span>
> * <span data-ttu-id="f46e3-109">Po rozšíření hello platí zobrazit spuštěné webu IIS</span><span class="sxs-lookup"><span data-stu-id="f46e3-109">View a running IIS site after hello extension is applied</span></span>

<span data-ttu-id="f46e3-110">Tento kurz vyžaduje hello prostředí Azure PowerShell verze modulu 3,6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f46e3-110">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="f46e3-111">Spustit ` Get-Module -ListAvailable AzureRM` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="f46e3-111">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="f46e3-112">Pokud potřebujete tooupgrade, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="f46e3-112">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>


## <a name="custom-script-extension-overview"></a><span data-ttu-id="f46e3-113">Přehled rozšíření vlastních skriptů</span><span class="sxs-lookup"><span data-stu-id="f46e3-113">Custom script extension overview</span></span>
<span data-ttu-id="f46e3-114">Hello rozšíření vlastních skriptů stahuje a spouští skripty na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="f46e3-114">hello Custom Script Extension downloads and executes scripts on Azure VMs.</span></span> <span data-ttu-id="f46e3-115">Toto rozšíření je užitečné pro konfiguraci nasazení post, instalace softwaru nebo jakoukoli jinou konfiguraci, nebo úlohu správy.</span><span class="sxs-lookup"><span data-stu-id="f46e3-115">This extension is useful for post deployment configuration, software installation, or any other configuration / management task.</span></span> <span data-ttu-id="f46e3-116">Skripty můžete stáhnout z úložiště Azure nebo GitHub nebo zadat toohello portál Azure na dobu běhu rozšíření.</span><span class="sxs-lookup"><span data-stu-id="f46e3-116">Scripts can be downloaded from Azure storage or GitHub, or provided toohello Azure portal at extension run time.</span></span>

<span data-ttu-id="f46e3-117">Hello rozšíření vlastních skriptů se integruje s šablon Azure Resource Manageru a můžete také spustit pomocí hello rozhraní příkazového řádku Azure, PowerShell, portálu Azure nebo hello REST API pro virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="f46e3-117">hello Custom Script extension integrates with Azure Resource Manager templates, and can also be run using hello Azure CLI, PowerShell, Azure portal, or hello Azure Virtual Machine REST API.</span></span>

<span data-ttu-id="f46e3-118">S Windows a virtuální počítače s Linuxem můžete hello rozšíření vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="f46e3-118">You can use hello Custom Script Extension with both Windows and Linux VMs.</span></span>


## <a name="create-virtual-machine"></a><span data-ttu-id="f46e3-119">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="f46e3-119">Create virtual machine</span></span>
<span data-ttu-id="f46e3-120">Před vytvořením virtuálního počítače, vytvořte skupinu prostředků s [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="f46e3-120">Before you can create a VM, create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="f46e3-121">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupAutomate* v hello *EastUS* umístění:</span><span class="sxs-lookup"><span data-stu-id="f46e3-121">hello following example creates a resource group named *myResourceGroupAutomate* in hello *EastUS* location:</span></span>

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupAutomate -Location EastUS
```

<span data-ttu-id="f46e3-122">Nastavte uživatelské jméno a heslo správce pro virtuální počítače hello s [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="f46e3-122">Set an administrator username and password for hello VMs with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="f46e3-123">Nyní můžete vytvořit virtuální počítač s hello [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="f46e3-123">Now you can create hello VM with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="f46e3-124">Hello následující příklad vytvoří hello požadované součásti virtuální sítě, konfiguraci hello operačního systému a poté vytvoří virtuální počítač s názvem *Můjvp*:</span><span class="sxs-lookup"><span data-stu-id="f46e3-124">hello following example creates hello required virtual network components, hello OS configuration, and then creates a VM named *myVM*:</span></span>

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

<span data-ttu-id="f46e3-125">Trvá několik minut, než hello prostředky a toobe virtuálních počítačů vytvořena.</span><span class="sxs-lookup"><span data-stu-id="f46e3-125">It takes a few minutes for hello resources and VM toobe created.</span></span>


## <a name="automate-iis-install"></a><span data-ttu-id="f46e3-126">Automatizaci instalace služby IIS</span><span class="sxs-lookup"><span data-stu-id="f46e3-126">Automate IIS install</span></span>
<span data-ttu-id="f46e3-127">Použití [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooinstall hello rozšíření vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="f46e3-127">Use [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooinstall hello Custom Script Extension.</span></span> <span data-ttu-id="f46e3-128">Hello rozšíření spustí `powershell Add-WindowsFeature Web-Server` tooinstall hello webový server služby IIS a poté aktualizace hello *Default.htm* stránky tooshow hello název hostitele hello virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="f46e3-128">hello extension runs `powershell Add-WindowsFeature Web-Server` tooinstall hello IIS webserver and then updates hello *Default.htm* page tooshow hello hostname of hello VM:</span></span>

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


## <a name="test-web-site"></a><span data-ttu-id="f46e3-129">Test webu</span><span class="sxs-lookup"><span data-stu-id="f46e3-129">Test web site</span></span>
<span data-ttu-id="f46e3-130">Získat hello veřejnou IP adresu nástroj pro vyrovnávání zatížení s [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="f46e3-130">Obtain hello public IP address of your load balancer with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="f46e3-131">Hello následující příklad získá hello IP adresu pro *myPublicIP* vytvořili dříve:</span><span class="sxs-lookup"><span data-stu-id="f46e3-131">hello following example obtains hello IP address for *myPublicIP* created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Name myPublicIP | select IpAddress
```

<span data-ttu-id="f46e3-132">Potom můžete zadat hello veřejnou IP adresu ve webovém prohlížeči tooa.</span><span class="sxs-lookup"><span data-stu-id="f46e3-132">You can then enter hello public IP address in tooa web browser.</span></span> <span data-ttu-id="f46e3-133">Hello web se zobrazuje, včetně hello název hostitele virtuálního počítače hello tento nástroj pro vyrovnávání zatížení hello distribuované tooas provoz v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="f46e3-133">hello website is displayed, including hello hostname of hello VM that hello load balancer distributed traffic tooas in hello following example:</span></span>

![Spuštění webu IIS](./media/tutorial-automate-vm-deployment/running-iis-website.png)


## <a name="next-steps"></a><span data-ttu-id="f46e3-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f46e3-135">Next steps</span></span>

<span data-ttu-id="f46e3-136">V tomto kurzu automatizované hello, který služba IIS nainstalovat na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="f46e3-136">In this tutorial, you automated hello IIS install on a VM.</span></span> <span data-ttu-id="f46e3-137">Naučili jste se tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="f46e3-137">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f46e3-138">Použít tooinstall hello rozšíření vlastních skriptů služby IIS</span><span class="sxs-lookup"><span data-stu-id="f46e3-138">Use hello Custom Script Extension tooinstall IIS</span></span>
> * <span data-ttu-id="f46e3-139">Vytvoření virtuálního počítače, který používá hello rozšíření vlastních skriptů</span><span class="sxs-lookup"><span data-stu-id="f46e3-139">Create a VM that uses hello Custom Script Extension</span></span>
> * <span data-ttu-id="f46e3-140">Po rozšíření hello platí zobrazit spuštěné webu IIS</span><span class="sxs-lookup"><span data-stu-id="f46e3-140">View a running IIS site after hello extension is applied</span></span>

<span data-ttu-id="f46e3-141">Jak zálohy další kurz toolearn toohello toocreate vlastní Image virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f46e3-141">Advance toohello next tutorial toolearn how toocreate custom VM images.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f46e3-142">Vytváření vlastních imagí virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="f46e3-142">Create custom VM images</span></span>](./tutorial-custom-images.md)
