---
title: "Rychlý Start - aaaAzure vytvořte VM prostředí PowerShell | Microsoft Docs"
description: "Naučte se rychle toocreate virtuální počítače s Linuxem pomocí prostředí PowerShell"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: f05ea7fedafe4fda660dc6084ae57ebf9dced473
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-virtual-machine-with-powershell"></a>Vytvoření virtuálního počítače s Linuxem pomocí PowerShellu

modul Azure PowerShell Hello je použité toocreate a spravovat prostředky Azure z příkazového řádku Powershellu hello nebo ve skriptech. Tento průvodce údaje pomocí toodeploy modulu Azure PowerShell hello virtuálního počítače s Ubuntu server. Po nasazení hello serveru se vytvoří připojení SSH a je nainstalován webovém serveru NGINX.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

Tento úvodní vyžaduje hello prostředí Azure PowerShell verze modulu 3,6 nebo novější. Spustit ` Get-Module -ListAvailable AzureRM` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).

Nakonec veřejný klíč SSH s názvem hello *id_rsa.pub* musí toobe uložené v hello *.ssh* adresář profil uživatele systému Windows. Podrobné informace o vytváření klíčů SSH pro Azure najdete v tématu popisujícím [vytvoření klíčů SSH pro Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="log-in-tooazure"></a>Přihlaste se tooAzure

Přihlaste se tooyour předplatné s hello `Login-AzureRmAccount` příkazů a postupujte podle hello na obrazovce pokynů.

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků Azure pomocí příkazu [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. 

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location eastus
```

## <a name="create-networking-resources"></a>Vytvoření síťových prostředků

Vytvořte virtuální síť, podsíť a veřejnou IP adresu. Tyto prostředky jsou použité tooprovide síťové připojení toohello virtuální počítač a připojte ho toohello Internetu.

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName myResourceGroup -Location eastus `
-Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup -Location eastus `
-AllocationMethod Static -IdleTimeoutInMinutes 4 -Name "mypublicdns$(Get-Random)"
```

Vytvořte skupinu zabezpečení sítě a pravidlo skupiny zabezpečení sítě. Skupina zabezpečení sítě Hello zabezpečuje hello virtuálního počítače pomocí příchozí a odchozí pravidla. V tomto případě se vytvoří příchozí pravidlo pro port 22, které umožní příchozí připojení SSH. Chceme také toocreate příchozí pravidlo pro port 80, která umožní příchozí webový provoz.

```powershell
# Create an inbound network security group rule for port 22
$nsgRuleSSH = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleSSH  -Protocol Tcp `
-Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 22 -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleWWW  -Protocol Tcp `
-Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName myResourceGroup -Location eastus `
-Name myNetworkSecurityGroup -SecurityRules $nsgRuleSSH,$nsgRuleWeb
```

Vytvoření síťové karty s [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) hello virtuálního počítače. Hello síťová karta připojí podsíť tooa hello virtuální počítač, skupina zabezpečení sítě a veřejnou IP adresu.

```powershell
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName myResourceGroup -Location eastus `
-SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-virtual-machine"></a>Vytvoření virtuálního počítače

Vytvořte konfiguraci virtuálního počítače. Tato konfigurace zahrnuje hello nastavení, které se použijí při nasazení virtuálního počítače hello třeba konfiguraci virtuálního počítače bitové kopie, velikost a ověřování.

```powershell
# Define a credential object
$securePassword = ConvertTo-SecureString ' ' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_D1 | `
Set-AzureRmVMOperatingSystem -Linux -ComputerName myVM -Credential $cred -DisablePasswordAuthentication | `
Set-AzureRmVMSourceImage -PublisherName Canonical -Offer UbuntuServer -Skus 14.04.2-LTS -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Configure SSH Keys
$sshPublicKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"
Add-AzureRmVMSshPublicKey -VM $vmconfig -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"
```

Vytvoření virtuálního počítače hello s [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroup -Location eastus -VM $vmConfig
```

## <a name="connect-toovirtual-machine"></a>Připojte počítač toovirtual

Po dokončení nasazení hello vytvoření připojení SSH s hello virtuálního počítače.

Použití hello [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) příkaz tooreturn hello veřejnou IP adresu hello virtuálního počítače.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

Ze systému pomocí protokolu SSH nainstalovaná hello použít následující příkaz tooconnect toohello virtuálního počítače. Pokud funguje v systému Windows, [Putty](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-ssh-from-windows?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-private-key-for-putty) lze použít toocreate hello připojení. 

```bash 
ssh <Public IP Address>
```

Po zobrazení výzvy hello přihlašovací uživatelské jméno je *azureuser*. Pokud přístupové heslo bylo zadáno při vytváření klíče SSH, musíte tuto také tooenter.


## <a name="install-nginx"></a>Instalace serveru NGINX

Použití hello následující zdroje balíčků tooupdate skriptů bash a instalovat nejnovější balíček NGINX hello. 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-hello-ngix-welcome-page"></a>Zobrazení hello NGIX úvodní stránka

NGINX nainstalován a port 80 nyní otevřete na vašem virtuálním počítači z hello Internetu – můžete použít webový prohlížeč volba tooview hello výchozí NGINX uvítací stránky. Být, že toouse hello veřejnou IP adresu, kterou popsané výše toovisit hello výchozí stránky. 

![Výchozí web NGINX](./media/quick-create-cli/nginx.png) 

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud již nepotřebujete, můžete použít hello [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) příkaz skupiny prostředků hello tooremove, virtuálních počítačů a všechny související prostředky.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

V tomto Rychlém startu jste nasadili jednoduchý virtuální počítač a pravidlo skupiny zabezpečení sítě a nainstalovali jste webový server. toolearn Další informace o virtuálních počítačích Azure, pokračovat v kurzu toohello pro virtuální počítače s Linuxem.

> [!div class="nextstepaction"]
> [Kurzy pro virtuální počítače Azure s Linuxem](./tutorial-manage-vm.md)
