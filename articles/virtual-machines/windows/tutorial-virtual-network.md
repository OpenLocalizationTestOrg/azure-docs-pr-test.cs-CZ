---
title: "aaaAzure virtuálních sítí a virtuální počítače s Windows | Microsoft Docs"
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
ms.openlocfilehash: ed77d9d5873e849fcb2aaf15e41899d7ad8c781a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-virtual-networks-and-windows-virtual-machines-with-azure-powershell"></a>Správa virtuálních sítí Azure a virtuální počítače s Windows pomocí Azure Powershellu

Virtuální počítače Azure pomocí Azure sítě pro vnitřní a vnější síťovou komunikaci. V tomto kurzu vytvoříte více virtuálních počítačů (VM) ve virtuální síti a konfigurace připojení k síti mezi nimi. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Vytvoření virtuální sítě
> * Vytvoření podsítě virtuální sítě
> * Ovládací prvek síťový provoz se skupinami zabezpečení sítě
> * Zobrazení pravidla pro provoz v akci

Tento kurz vyžaduje hello prostředí Azure PowerShell verze modulu 3,6 nebo novější. Spustit ` Get-Module -ListAvailable AzureRM` toofind hello verze. Pokud potřebujete tooupgrade, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="create-vnet"></a>Vytvoření sítě VNet

Virtuální síť je reprezentace vlastní sítě v cloudu hello. Virtuální síť je to logická izolace cloudu Azure vyhrazeného tooyour předplatné hello. V rámci virtuální sítě zjistíte, podsítě, pravidla pro podsítě toothose připojení a připojení z podsítě toohello hello virtuálních počítačů.

Před vytvořením veškeré prostředky Azure, je nutné toocreate skupinu prostředků s [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Hello následující příklad vytvoří skupinu prostředků s názvem *myRGNetwork* v hello *EastUS* umístění:

```powershell
New-AzureRmResourceGroup -ResourceGroupName myRGNetwork -Location EastUS
```

Podsíť je prostředkem podřízené virtuální sítě, a pomáhá definovat segmenty adresní prostory v rámci blok CIDR pomocí předpony IP adres. Síťové adaptéry lze přidat toosubnets a připojené tooVMs, poskytuje připojení pro různé úlohy.

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

Pro virtuální počítač toocommunicate ve virtuální síti musí být virtuální síťové rozhraní (NIC). Hello *myFrontendVM* je k němu přistupovat z hello Internetu, takže je také nutné veřejnou IP adresu. 

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

Nastavte hello uživatelské jméno a heslo, které potřebuje pro účet správce hello na hello virtuálních počítačů s [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Vytvoření hello virtuálních počítačů s [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk), [Přidat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface), a [nové AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). 

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

Nainstalovat službu IIS na *myFrontendVM* pomocí relaci vzdálené plochy. Je třeba tooget hello veřejnou IP adresu hello tooaccess virtuálního počítače jej.

Můžete získat hello veřejnou IP adresu *myFrontendVM* s [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Hello následující příklad získá hello IP adresu pro *myPublicIPAddress* vytvořili dříve:

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName myRGNetwork `
    -Name myPublicIPAddress | select IpAddress
```

Poznamenejte si tuto IP adresu, můžete ji použít v budoucnu krocích.

Použití hello následující příkaz toocreate a relace vzdálené plochy s *myFrontendVM*. Nahraďte  *<publicIPAddress>*  hello adresy, která jste si předtím poznamenali. Po zobrazení výzvy zadejte přihlašovací údaje hello při vytvoření hello virtuálních počítačů.

```
mstsc /v:<publicIpAddress>
``` 

Teď, když jste přihlášení příliš*myFrontendVM*, můžete použít jeden řádek tooinstall prostředí PowerShell služby IIS a povolit hello místní brány firewall pravidla tooallow webový provoz. Otevřete příkazovém řádku prostředí PowerShell a spusťte následující příkaz hello:

Použití [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/servermanager/install-windowsfeature) rozšíření vlastních skriptů hello toorun, který instaluje webový server IIS hello:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Teď můžete použít hello veřejnou IP adresu toobrowse webu IIS toohello virtuálních počítačů toosee hello.

![Výchozí web služby IIS](./media/tutorial-virtual-network/iis.png)

## <a name="manage-internal-traffic"></a>Spravovat interní provoz

Skupina zabezpečení sítě (NSG) obsahuje seznam pravidel zabezpečení, která povolují nebo odepírají síťový provoz tooresources připojené tooa virtuální sítě. Skupiny Nsg můžou být přidružené toosubnets nebo jednotlivé síťové adaptéry připojené tooVMs. Otevírání nebo při zavření tooVMs přístup prostřednictvím portů se provádí pomocí pravidla NSG. Při vytváření *myFrontendVM*, příchozí port 3389 se automaticky otevře pro připojení RDP.

Interní komunikaci virtuálních počítačů můžete konfigurovat pomocí skupinu NSG. V této části se dozvíte, jak toocreate podsíť další v hello sítě a přiřadit tooallow tooit NSG připojení z *myFrontendVM* příliš*myBackendVM* na portu 1433. podsíť Hello byl přiřazen toohello virtuálního počítače při jeho vytvoření.

Můžete omezit přenosy interní příliš*myBackendVM* z jenom *myFrontendVM* tak, že vytvoříte skupinu NSG pro podsíť hello back-end. Hello následující příklad vytvoří pravidlo NSG s názvem *myBackendNSGRule* s [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig):

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

Přidat *myBackEndSubnet* příliš*myVNet* s [přidat AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig):

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

Hello nejjednodušší způsob, jak toocreate hello back-end virtuální počítač pomocí bitové kopie systému SQL Server. V tomto kurzu jenom vytvoří hello virtuálních počítačů s hello databázový server, ale neposkytuje informace o přístup k databázi hello.

Vytvoření *myBackendNic*:

```powershell
$backendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNic `
  -SubnetId $vnet.Subnets[1].Id
```

Nastavte hello uživatelské jméno a heslo pro účet správce hello na hello virtuálního počítače s Get-Credential potřebné:

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

Hello bitovou kopii, která se používá nainstalován server SQL, ale není použit v tomto kurzu. Je součástí tooshow můžete konfiguraci aplikace virtuálních počítačů toohandle webového provozu a správy virtuálních počítačů toohandle databáze.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste vytvořili a zabezpečené sítě Azure jako související toovirtual počítače. 

> [!div class="checklist"]
> * Vytvoření virtuální sítě
> * Vytvoření podsítě virtuální sítě
> * Ovládací prvek síťový provoz se skupinami zabezpečení sítě
> * Zobrazení pravidla pro provoz v akci

Posunutí další kurz toolearn toohello o monitorování zabezpečení dat na virtuální počítače pomocí zálohování Azure. .

> [!div class="nextstepaction"]
> [Zálohovat virtuální počítače s Windows v Azure](./tutorial-backup-vms.md)
