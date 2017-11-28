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
# <a name="create-a-linux-virtual-machine-with-powershell"></a><span data-ttu-id="f61e4-103">Vytvoření virtuálního počítače s Linuxem pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="f61e4-103">Create a Linux virtual machine with PowerShell</span></span>

<span data-ttu-id="f61e4-104">modul Azure PowerShell Hello je použité toocreate a spravovat prostředky Azure z příkazového řádku Powershellu hello nebo ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="f61e4-104">hello Azure PowerShell module is used toocreate and manage Azure resources from hello PowerShell command line or in scripts.</span></span> <span data-ttu-id="f61e4-105">Tento průvodce údaje pomocí toodeploy modulu Azure PowerShell hello virtuálního počítače s Ubuntu server.</span><span class="sxs-lookup"><span data-stu-id="f61e4-105">This guide details using hello Azure PowerShell module toodeploy a virtual machine running Ubuntu server.</span></span> <span data-ttu-id="f61e4-106">Po nasazení hello serveru se vytvoří připojení SSH a je nainstalován webovém serveru NGINX.</span><span class="sxs-lookup"><span data-stu-id="f61e4-106">Once hello server is deployed, an SSH connection is created, and an NGINX webserver is installed.</span></span>

<span data-ttu-id="f61e4-107">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="f61e4-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

<span data-ttu-id="f61e4-108">Tento úvodní vyžaduje hello prostředí Azure PowerShell verze modulu 3,6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f61e4-108">This quick start requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="f61e4-109">Spustit ` Get-Module -ListAvailable AzureRM` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="f61e4-109">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="f61e4-110">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="f61e4-110">If you need tooinstall or upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

<span data-ttu-id="f61e4-111">Nakonec veřejný klíč SSH s názvem hello *id_rsa.pub* musí toobe uložené v hello *.ssh* adresář profil uživatele systému Windows.</span><span class="sxs-lookup"><span data-stu-id="f61e4-111">Finally, a public SSH key with hello name *id_rsa.pub* needs toobe stored in hello *.ssh* directory of your Windows user profile.</span></span> <span data-ttu-id="f61e4-112">Podrobné informace o vytváření klíčů SSH pro Azure najdete v tématu popisujícím [vytvoření klíčů SSH pro Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f61e4-112">For detailed information on creating SSH keys for Azure, see [Create SSH keys for Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="f61e4-113">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="f61e4-113">Log in tooAzure</span></span>

<span data-ttu-id="f61e4-114">Přihlaste se tooyour předplatné s hello `Login-AzureRmAccount` příkazů a postupujte podle hello na obrazovce pokynů.</span><span class="sxs-lookup"><span data-stu-id="f61e4-114">Log in tooyour Azure subscription with hello `Login-AzureRmAccount` command and follow hello on-screen directions.</span></span>

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a><span data-ttu-id="f61e4-115">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="f61e4-115">Create resource group</span></span>

<span data-ttu-id="f61e4-116">Vytvořte skupinu prostředků Azure pomocí příkazu [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="f61e4-116">Create an Azure resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="f61e4-117">Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="f61e4-117">A resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location eastus
```

## <a name="create-networking-resources"></a><span data-ttu-id="f61e4-118">Vytvoření síťových prostředků</span><span class="sxs-lookup"><span data-stu-id="f61e4-118">Create networking resources</span></span>

<span data-ttu-id="f61e4-119">Vytvořte virtuální síť, podsíť a veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="f61e4-119">Create a virtual network, subnet, and a public IP address.</span></span> <span data-ttu-id="f61e4-120">Tyto prostředky jsou použité tooprovide síťové připojení toohello virtuální počítač a připojte ho toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="f61e4-120">These resources are used tooprovide network connectivity toohello virtual machine and connect it toohello internet.</span></span>

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

<span data-ttu-id="f61e4-121">Vytvořte skupinu zabezpečení sítě a pravidlo skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="f61e4-121">Create a network security group and a network security group rule.</span></span> <span data-ttu-id="f61e4-122">Skupina zabezpečení sítě Hello zabezpečuje hello virtuálního počítače pomocí příchozí a odchozí pravidla.</span><span class="sxs-lookup"><span data-stu-id="f61e4-122">hello network security group secures hello virtual machine using inbound and outbound rules.</span></span> <span data-ttu-id="f61e4-123">V tomto případě se vytvoří příchozí pravidlo pro port 22, které umožní příchozí připojení SSH.</span><span class="sxs-lookup"><span data-stu-id="f61e4-123">In this case, an inbound rule is created for port 22, which allows incoming SSH connections.</span></span> <span data-ttu-id="f61e4-124">Chceme také toocreate příchozí pravidlo pro port 80, která umožní příchozí webový provoz.</span><span class="sxs-lookup"><span data-stu-id="f61e4-124">We also want toocreate an inbound rule for port 80, which allows incoming web traffic.</span></span>

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

<span data-ttu-id="f61e4-125">Vytvoření síťové karty s [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f61e4-125">Create a network card with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) for hello virtual machine.</span></span> <span data-ttu-id="f61e4-126">Hello síťová karta připojí podsíť tooa hello virtuální počítač, skupina zabezpečení sítě a veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="f61e4-126">hello network card connects hello virtual machine tooa subnet, network security group, and public IP address.</span></span>

```powershell
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName myResourceGroup -Location eastus `
-SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-virtual-machine"></a><span data-ttu-id="f61e4-127">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="f61e4-127">Create virtual machine</span></span>

<span data-ttu-id="f61e4-128">Vytvořte konfiguraci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f61e4-128">Create a virtual machine configuration.</span></span> <span data-ttu-id="f61e4-129">Tato konfigurace zahrnuje hello nastavení, které se použijí při nasazení virtuálního počítače hello třeba konfiguraci virtuálního počítače bitové kopie, velikost a ověřování.</span><span class="sxs-lookup"><span data-stu-id="f61e4-129">This configuration includes hello settings that are used when deploying hello virtual machine such as a virtual machine image, size, and authentication configuration.</span></span>

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

<span data-ttu-id="f61e4-130">Vytvoření virtuálního počítače hello s [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="f61e4-130">Create hello virtual machine with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span>

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroup -Location eastus -VM $vmConfig
```

## <a name="connect-toovirtual-machine"></a><span data-ttu-id="f61e4-131">Připojte počítač toovirtual</span><span class="sxs-lookup"><span data-stu-id="f61e4-131">Connect toovirtual machine</span></span>

<span data-ttu-id="f61e4-132">Po dokončení nasazení hello vytvoření připojení SSH s hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f61e4-132">After hello deployment has completed, create an SSH connection with hello virtual machine.</span></span>

<span data-ttu-id="f61e4-133">Použití hello [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) příkaz tooreturn hello veřejnou IP adresu hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f61e4-133">Use hello [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) command tooreturn hello public IP address of hello virtual machine.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

<span data-ttu-id="f61e4-134">Ze systému pomocí protokolu SSH nainstalovaná hello použít následující příkaz tooconnect toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f61e4-134">From a system with SSH installed, used hello following command tooconnect toohello virtual machine.</span></span> <span data-ttu-id="f61e4-135">Pokud funguje v systému Windows, [Putty](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-ssh-from-windows?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-private-key-for-putty) lze použít toocreate hello připojení.</span><span class="sxs-lookup"><span data-stu-id="f61e4-135">If working on Windows, [Putty](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-ssh-from-windows?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-private-key-for-putty) can be used toocreate hello connection.</span></span> 

```bash 
ssh <Public IP Address>
```

<span data-ttu-id="f61e4-136">Po zobrazení výzvy hello přihlašovací uživatelské jméno je *azureuser*.</span><span class="sxs-lookup"><span data-stu-id="f61e4-136">When prompted, hello login user name is *azureuser*.</span></span> <span data-ttu-id="f61e4-137">Pokud přístupové heslo bylo zadáno při vytváření klíče SSH, musíte tuto také tooenter.</span><span class="sxs-lookup"><span data-stu-id="f61e4-137">If a passphrase was entered when creating SSH keys, you need tooenter this as well.</span></span>


## <a name="install-nginx"></a><span data-ttu-id="f61e4-138">Instalace serveru NGINX</span><span class="sxs-lookup"><span data-stu-id="f61e4-138">Install NGINX</span></span>

<span data-ttu-id="f61e4-139">Použití hello následující zdroje balíčků tooupdate skriptů bash a instalovat nejnovější balíček NGINX hello.</span><span class="sxs-lookup"><span data-stu-id="f61e4-139">Use hello following bash script tooupdate package sources and install hello latest NGINX package.</span></span> 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-hello-ngix-welcome-page"></a><span data-ttu-id="f61e4-140">Zobrazení hello NGIX úvodní stránka</span><span class="sxs-lookup"><span data-stu-id="f61e4-140">View hello NGIX welcome page</span></span>

<span data-ttu-id="f61e4-141">NGINX nainstalován a port 80 nyní otevřete na vašem virtuálním počítači z hello Internetu – můžete použít webový prohlížeč volba tooview hello výchozí NGINX uvítací stránky.</span><span class="sxs-lookup"><span data-stu-id="f61e4-141">With NGINX installed and port 80 now open on your VM from hello Internet - you can use a web browser of your choice tooview hello default NGINX welcome page.</span></span> <span data-ttu-id="f61e4-142">Být, že toouse hello veřejnou IP adresu, kterou popsané výše toovisit hello výchozí stránky.</span><span class="sxs-lookup"><span data-stu-id="f61e4-142">Be sure toouse hello public IP address you documented above toovisit hello default page.</span></span> 

![Výchozí web NGINX](./media/quick-create-cli/nginx.png) 

## <a name="clean-up-resources"></a><span data-ttu-id="f61e4-144">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="f61e4-144">Clean up resources</span></span>

<span data-ttu-id="f61e4-145">Pokud již nepotřebujete, můžete použít hello [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) příkaz skupiny prostředků hello tooremove, virtuálních počítačů a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="f61e4-145">When no longer needed, you can use hello [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="f61e4-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f61e4-146">Next steps</span></span>

<span data-ttu-id="f61e4-147">V tomto Rychlém startu jste nasadili jednoduchý virtuální počítač a pravidlo skupiny zabezpečení sítě a nainstalovali jste webový server.</span><span class="sxs-lookup"><span data-stu-id="f61e4-147">In this quick start, you’ve deployed a simple virtual machine, a network security group rule, and installed a web server.</span></span> <span data-ttu-id="f61e4-148">toolearn Další informace o virtuálních počítačích Azure, pokračovat v kurzu toohello pro virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="f61e4-148">toolearn more about Azure virtual machines, continue toohello tutorial for Linux VMs.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f61e4-149">Kurzy pro virtuální počítače Azure s Linuxem</span><span class="sxs-lookup"><span data-stu-id="f61e4-149">Azure Linux virtual machine tutorials</span></span>](./tutorial-manage-vm.md)
