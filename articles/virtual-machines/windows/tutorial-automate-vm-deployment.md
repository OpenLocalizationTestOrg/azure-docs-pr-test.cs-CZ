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
# <a name="how-toocustomize-a-windows-virtual-machine-in-azure"></a>Jak toocustomize virtuálního počítače s Windows v Azure
Obvykle se požaduje tooconfigure virtuální počítače (VM) rychlé a konzistentním způsobem, nějaký způsob automatizace. Běžné toocustomize přístup virtuální počítač s Windows je toouse [vlastní skript rozšíření pro Windows](extensions-customscript.md). V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Použít tooinstall hello rozšíření vlastních skriptů služby IIS
> * Vytvoření virtuálního počítače, který používá hello rozšíření vlastních skriptů
> * Po rozšíření hello platí zobrazit spuštěné webu IIS

Tento kurz vyžaduje hello prostředí Azure PowerShell verze modulu 3,6 nebo novější. Spustit ` Get-Module -ListAvailable AzureRM` toofind hello verze. Pokud potřebujete tooupgrade, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).


## <a name="custom-script-extension-overview"></a>Přehled rozšíření vlastních skriptů
Hello rozšíření vlastních skriptů stahuje a spouští skripty na virtuálních počítačích Azure. Toto rozšíření je užitečné pro konfiguraci nasazení post, instalace softwaru nebo jakoukoli jinou konfiguraci, nebo úlohu správy. Skripty můžete stáhnout z úložiště Azure nebo GitHub nebo zadat toohello portál Azure na dobu běhu rozšíření.

Hello rozšíření vlastních skriptů se integruje s šablon Azure Resource Manageru a můžete také spustit pomocí hello rozhraní příkazového řádku Azure, PowerShell, portálu Azure nebo hello REST API pro virtuální počítač Azure.

S Windows a virtuální počítače s Linuxem můžete hello rozšíření vlastních skriptů.


## <a name="create-virtual-machine"></a>Vytvoření virtuálního počítače
Před vytvořením virtuálního počítače, vytvořte skupinu prostředků s [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupAutomate* v hello *EastUS* umístění:

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupAutomate -Location EastUS
```

Nastavte uživatelské jméno a heslo správce pro virtuální počítače hello s [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Nyní můžete vytvořit virtuální počítač s hello [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Hello následující příklad vytvoří hello požadované součásti virtuální sítě, konfiguraci hello operačního systému a poté vytvoří virtuální počítač s názvem *Můjvp*:

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

Trvá několik minut, než hello prostředky a toobe virtuálních počítačů vytvořena.


## <a name="automate-iis-install"></a>Automatizaci instalace služby IIS
Použití [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooinstall hello rozšíření vlastních skriptů. Hello rozšíření spustí `powershell Add-WindowsFeature Web-Server` tooinstall hello webový server služby IIS a poté aktualizace hello *Default.htm* stránky tooshow hello název hostitele hello virtuálních počítačů:

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


## <a name="test-web-site"></a>Test webu
Získat hello veřejnou IP adresu nástroj pro vyrovnávání zatížení s [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Hello následující příklad získá hello IP adresu pro *myPublicIP* vytvořili dříve:

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Name myPublicIP | select IpAddress
```

Potom můžete zadat hello veřejnou IP adresu ve webovém prohlížeči tooa. Hello web se zobrazuje, včetně hello název hostitele virtuálního počítače hello tento nástroj pro vyrovnávání zatížení hello distribuované tooas provoz v hello následující ukázka:

![Spuštění webu IIS](./media/tutorial-automate-vm-deployment/running-iis-website.png)


## <a name="next-steps"></a>Další kroky

V tomto kurzu automatizované hello, který služba IIS nainstalovat na virtuálním počítači. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Použít tooinstall hello rozšíření vlastních skriptů služby IIS
> * Vytvoření virtuálního počítače, který používá hello rozšíření vlastních skriptů
> * Po rozšíření hello platí zobrazit spuštěné webu IIS

Jak zálohy další kurz toolearn toohello toocreate vlastní Image virtuálních počítačů.

> [!div class="nextstepaction"]
> [Vytváření vlastních imagí virtuálních počítačů](./tutorial-custom-images.md)
