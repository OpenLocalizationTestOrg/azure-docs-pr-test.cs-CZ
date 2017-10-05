---
title: "Virtuální sítě Azure a virtuální počítače s Windows | Microsoft Docs"
description: "Kurz – Správa virtuálních sítí Azure a virtuální počítače s Windows pomocí Azure Powershellu"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: c71c07f8ecd123a7e27848ba5043d46e315fcf03
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="manage-azure-virtual-networks-and-windows-virtual-machines-with-azure-powershell"></a>Správa virtuálních sítí Azure a virtuální počítače s Windows pomocí Azure Powershellu

Virtuální počítače Azure pomocí Azure sítě pro vnitřní a vnější síťovou komunikaci. V tomto kurzu vytvoříte více virtuálních počítačů (VM) ve virtuální síti a konfigurace připojení k síti mezi nimi. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Vytvoření virtuální sítě
> * Vytvoření podsítě virtuální sítě
> * Ovládací prvek síťový provoz se skupinami zabezpečení sítě
> * Zobrazení pravidla pro provoz v akci

Tento kurz vyžaduje modul Azure PowerShell verze 3.6 nebo novější. Verzi zjistíte spuštěním příkazu ` Get-Module -ListAvailable AzureRM`. Pokud je třeba upgradovat, přečtěte si téma [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="create-vnet"></a>Vytvoření sítě VNet

Virtuální síť je reprezentace vlastní sítě v cloudu. Virtuální síť je to logická izolace cloudu Azure vyhrazeného pro vaše předplatné. V rámci virtuální sítě zjistíte, podsítě, pravidla pro připojení k těchto podsítí a připojení z virtuálních počítačů k podsítím.

Před vytvořením veškeré prostředky Azure, je nutné vytvořit skupinu prostředků s [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Následující příklad vytvoří skupinu prostředků s názvem *myRGNetwork* v *EastUS* umístění:

```powershell
New-AzureRmResourceGroup -ResourceGroupName myRGNetwork -Location EastUS
```

Podsíť je prostředkem podřízené virtuální sítě, a pomáhá definovat segmenty adresní prostory v rámci blok CIDR pomocí předpony IP adres. Síťové adaptéry můžete přidat do podsítí a připojení k virtuálním počítačům, poskytuje připojení pro různé úlohy.

Vytvořte podsíť s [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):

```powershell
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myFrontendSubnet `
  -AddressPrefix 10.0.0.0/24
```

Vytvoření virtuální sítě s názvem *myVNet* pomocí *myFrontendSubnet* s [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $frontendSubnet
```

## <a name="create-front-end-vm"></a>Vytvoření virtuálního počítače s front-endu

Pro virtuální počítač pro komunikaci ve virtuální síti musí být virtuální síťové rozhraní (NIC). *MyFrontendVM* přístupná z Internetu, takže je také nutné veřejnou IP adresu. 

Vytvoření veřejné IP adresy s [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):

```powershell
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIPAddress
```

Vytvořit síťový adaptér s [nové AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):


```powershell
$frontendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontendNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

Nastavte uživatelské jméno a heslo potřebné pro správce účtu na virtuálním počítači s [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Vytvoření virtuálních počítačů s [nové AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk), [přidat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface), a [nové AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). 

```powershell
$frontendVM = New-AzureRmVMConfig `
    -VMName myFrontendVM `
    -VMSize Standard_D1
$frontendVM = Set-AzureRmVMOperatingSystem `
    -VM $frontendVM `
    -Windows `
    -ComputerName myFrontendVM `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
$frontendVM = Set-AzureRmVMSourceImage `
    -VM $frontendVM `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
$frontendVM = Set-AzureRmVMOSDisk `
    -VM $frontendVM `
    -Name myFrontendOSDisk `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
$frontendVM = Add-AzureRmVMNetworkInterface `
    -VM $frontendVM `
    -Id $frontendNic.Id
New-AzureRmVM `
    -ResourceGroupName myRGNetwork `
    -Location EastUS `
    -VM $frontendVM
```

## <a name="install-web-server"></a>Instalace webového serveru

Nainstalovat službu IIS na *myFrontendVM* pomocí relaci vzdálené plochy. Budete muset získat veřejnou IP adresu virtuálního počítače k přístupu.

Můžete získat veřejnou IP adresu *myFrontendVM* s [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Následující příklad, získá IP adresu pro *myPublicIPAddress* vytvořili dříve:

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName myRGNetwork `
    -Name myPublicIPAddress | select IpAddress
```

Poznamenejte si tuto IP adresu, můžete ji použít v budoucnu krocích.

Použijte následující příkaz Vytvořit relaci vzdálené plochy s *myFrontendVM*. Nahraďte  *<publicIPAddress>*  adresy, která jste si předtím poznamenali. Po zobrazení výzvy zadejte přihlašovací údaje použité při vytváření virtuálního počítače.

```
mstsc /v:<publicIpAddress>
``` 

Teď, když jste přihlášení k *myFrontendVM*, jeden řádek prostředí PowerShell můžete použít k instalaci IIS a aktivace pravidla místní brány firewall pro povolení webový provoz. Otevřete příkazový řádek PowerShellu a spusťte následující příkaz:

Použití [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/servermanager/install-windowsfeature) ke spuštění rozšíření vlastních skriptů, který instaluje webový server služby IIS:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Teď můžete použít veřejnou IP adresu a přejděte do virtuálního počítače najdete v části webu služby IIS.

![Výchozí web služby IIS](./media/tutorial-virtual-network/iis.png)

## <a name="manage-internal-traffic"></a>Spravovat interní provoz

Skupina zabezpečení sítě (NSG) obsahuje seznam pravidel zabezpečení, která povolují nebo odpírají síťový provoz na prostředky, které jsou připojené k virtuální síti. Skupiny Nsg můžou být přidružena k podsítě nebo jednotlivé síťové adaptéry připojené k virtuálním počítačům. Otevírání nebo při zavření přístup k virtuálním počítačům prostřednictvím portů se provádí pomocí pravidla NSG. Při vytváření *myFrontendVM*, příchozí port 3389 se automaticky otevře pro připojení RDP.

Interní komunikaci virtuálních počítačů můžete konfigurovat pomocí skupinu NSG. V této části se dozvíte, jak vytvořit další podsítě v síti a přiřadit skupinu NSG k němu povolit připojení z *myFrontendVM* k *myBackendVM* na portu 1433. Podsíť je pak přiřazené k virtuálnímu počítači při jeho vytvoření.

Můžete omezit interní provoz *myBackendVM* z jenom *myFrontendVM* tak, že vytvoříte skupinu NSG pro podsíť back-end. Následující příklad vytvoří pravidlo NSG s názvem *myBackendNSGRule* s [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig):

```powershell
$nsgBackendRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myBackendNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 100 `
  -SourceAddressPrefix 10.0.0.0/24 `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 1433 `
  -Access Allow
```

Přidat skupinu zabezpečení sítě s názvem *myBackendNSG* s [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):

```powershell
$nsgBackend = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNSG `
  -SecurityRules $nsgBackendRule
```
## <a name="add-back-end-subnet"></a>Přidat podsíť back-end

Přidat *myBackEndSubnet* k *myVNet* s [přidat AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig):

```powershell
Add-AzureRmVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -VirtualNetwork $vnet `
  -AddressPrefix 10.0.1.0/24 `
  -NetworkSecurityGroup $nsgBackend
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Name myVNet
```

## <a name="create-back-end-vm"></a>Vytvoření virtuálního počítače s back-end

Nejjednodušší způsob, jak vytvořit virtuální počítač back-end je pomocí bitové kopie systému SQL Server. V tomto kurzu pouze vytvoří virtuální počítač s databázovým serverem, ale neposkytuje informace o přístupu k databázi.

Vytvoření *myBackendNic*:

```powershell
$backendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNic `
  -SubnetId $vnet.Subnets[1].Id
```

Nastavte uživatelské jméno a heslo pro účet správce ve virtuálním počítači s Get-Credential potřebné:

```powershell
$cred = Get-Credential
```

Vytvoření *myBackendVM*:

```powershell
$backendVM = New-AzureRmVMConfig `
  -VMName myBackendVM `
  -VMSize Standard_D1
$backendVM = Set-AzureRmVMOperatingSystem `
  -VM $backendVM `
  -Windows `
  -ComputerName myBackendVM `
  -Credential $cred `
  -ProvisionVMAgent `
  -EnableAutoUpdate
$backendVM = Set-AzureRmVMSourceImage `
  -VM $backendVM `
  -PublisherName MicrosoftSQLServer `
  -Offer SQL2016SP1-WS2016 `
  -Skus Enterprise `
  -Version latest
$backendVM = Set-AzureRmVMOSDisk `
  -VM $backendVM `
  -Name myBackendOSDisk `
  -DiskSizeInGB 128 `
  -CreateOption FromImage `
  -Caching ReadWrite
$backendVM = Add-AzureRmVMNetworkInterface `
  -VM $backendVM `
  -Id $backendNic.Id
New-AzureRmVM `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -VM $backendVM
```

Obrázek, který je použit nainstalován server SQL, ale není použit v tomto kurzu. Je zahrnutá ukázat vám, jak můžete nakonfigurovat virtuální počítač pro zpracování webových přenosů a virtuální počítač pro správu databáze.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste vytvořili a zabezpečené sítě v souvislosti s virtuálními počítači Azure. 

> [!div class="checklist"]
> * Vytvoření virtuální sítě
> * Vytvoření podsítě virtuální sítě
> * Ovládací prvek síťový provoz se skupinami zabezpečení sítě
> * Zobrazení pravidla pro provoz v akci

Přechodu na v dalším kurzu se dozvíte o monitorování zabezpečení dat na virtuální počítače pomocí zálohování Azure. .

> [!div class="nextstepaction"]
> [Zálohovat virtuální počítače s Windows v Azure](./tutorial-backup-vms.md)
