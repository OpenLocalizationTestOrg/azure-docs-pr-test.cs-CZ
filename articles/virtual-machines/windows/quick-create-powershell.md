---
title: "Rychlý Start - aaaAzure prostředí PowerShell vytvořit virtuální počítač Windows | Microsoft Docs"
description: "Naučte se rychle toocreate virtuální počítače s Windows pomocí prostředí PowerShell"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 5e92435bf7ee443a01c158fed91c356363e2f425
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-virtual-machine-with-powershell"></a>Vytvoření virtuálního počítače s Windows pomocí PowerShellu

modul Azure PowerShell Hello je použité toocreate a spravovat prostředky Azure z příkazového řádku Powershellu hello nebo ve skriptech. Tato příručka údaje pomocí prostředí PowerShell toocreate a Azure virtuální počítač se systémem Windows Server 2016. Po dokončení nasazení jsme připojení toohello serveru a instalace služby IIS.  

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

Tento úvodní vyžaduje hello prostředí Azure PowerShell verze modulu 3,6 nebo novější. Spustit ` Get-Module -ListAvailable AzureRM` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="log-in-tooazure"></a>Přihlaste se tooAzure

Přihlaste se tooyour předplatné s hello `Login-AzureRmAccount` příkazů a postupujte podle hello na obrazovce pokynů.

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků Azure pomocí příkazu [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. 

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-networking-resources"></a>Vytvoření síťových prostředků

### <a name="create-a-virtual-network-subnet-and-a-public-ip-address"></a>Vytvořte virtuální síť, podsíť a veřejnou IP adresu. 
Tyto prostředky jsou použité tooprovide síťové připojení toohello virtuální počítač a připojte ho toohello Internetu.

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName myResourceGroup -Location EastUS `
    -Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup -Location EastUS `
    -AllocationMethod Static -IdleTimeoutInMinutes 4 -Name "mypublicdns$(Get-Random)"
```

### <a name="create-a-network-security-group-and-a-network-security-group-rule"></a>Vytvořte skupinu zabezpečení sítě a pravidlo skupiny zabezpečení sítě. 
Skupina zabezpečení sítě Hello zabezpečuje hello virtuálního počítače pomocí příchozí a odchozí pravidla. V tomto případě se vytvoří příchozí pravidlo pro port 3389, které umožní příchozí připojení ke vzdálené ploše. Chceme také toocreate příchozí pravidlo pro port 80, která umožní příchozí webový provoz.

```powershell
# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
    -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
    -DestinationPortRange 3389 -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleWWW  -Protocol Tcp `
    -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
    -DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName myResourceGroup -Location EastUS `
    -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP,$nsgRuleWeb
```

### <a name="create-a-network-card-for-hello-virtual-machine"></a>Vytvořte síťové karty pro virtuální počítač hello. 
Vytvoření síťové karty s [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) hello virtuálního počítače. Hello síťová karta připojí podsíť tooa hello virtuální počítač, skupina zabezpečení sítě a veřejnou IP adresu.

```powershell
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName myResourceGroup -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-virtual-machine"></a>Vytvoření virtuálního počítače

Vytvořte konfiguraci virtuálního počítače. Tato konfigurace zahrnuje hello nastavení, které se použijí při nasazení virtuálního počítače hello třeba konfiguraci virtuálního počítače bitové kopie, velikost a ověřování. Při spuštění tohoto kroku se zobrazí výzva k zadání přihlašovacích údajů. Hello hodnoty, které zadáte, jsou nakonfigurovány jako hello uživatelské jméno a heslo pro virtuální počítač hello.

```powershell
# Define a credential object
$cred = Get-Credential

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS2 | `
    Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
    Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer `
    -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nic.Id
```

Vytvoření virtuálního počítače hello s [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroup -Location EastUS -VM $vmConfig
```

## <a name="connect-toovirtual-machine"></a>Připojte počítač toovirtual

Po dokončení nasazení hello vytvořte připojení ke vzdálené ploše se hello virtuálního počítače.

Použití hello [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) příkaz tooreturn hello veřejnou IP adresu hello virtuálního počítače. Poznamenejte si tuto IP adresu, tooit můžete propojit s váš prohlížeč tootest připojení k webu v příštím kroku.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

Použití hello následující příkaz toocreate a relace vzdálené plochy s hello virtuálního počítače. Nahraďte IP adresu hello hello *publicIPAddress* virtuálního počítače. Po zobrazení výzvy zadejte přihlašovací údaje hello použité při vytváření hello virtuálního počítače.

```bash 
mstsc /v:<publicIpAddress>
```

## <a name="install-iis-via-powershell"></a>Instalace služby IIS pomocí PowerShellu

Teď, když jste byli přihlášeni toohello virtuálního počítače Azure, můžete použít jeden řádek tooinstall prostředí PowerShell služby IIS a povolit hello místní brány firewall pravidla tooallow webový provoz. Otevřete příkazovém řádku prostředí PowerShell a spusťte následující příkaz hello:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-hello-iis-welcome-page"></a>Zobrazení hello úvodní stránka služby IIS

S nainstalovanou službu IIS a port 80 nyní otevřete na vašem virtuálním počítači z hello Internetu můžete použít webový prohlížeč choice tooview hello výchozí IIS uvítací stránky. Se, zda text hello toouse *publicIpAddress* popsané výše toovisit hello výchozí stránky. 

![Výchozí web služby IIS](./media/quick-create-powershell/default-iis-website.png) 

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud již nepotřebujete, můžete použít hello [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) příkaz skupiny prostředků hello tooremove, virtuálních počítačů a všechny související prostředky.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

V tomto Rychlém startu jste nasadili jednoduchý virtuální počítač a pravidlo skupiny zabezpečení sítě a nainstalovali jste webový server. toolearn Další informace o virtuálních počítačích Azure, pokračovat v kurzu toohello pro virtuální počítače Windows.

> [!div class="nextstepaction"]
> [Kurzy pro virtuální počítače Azure s Windows](./tutorial-manage-vm.md)
