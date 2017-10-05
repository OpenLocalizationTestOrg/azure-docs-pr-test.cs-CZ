---
title: "Rychlý start Azure – Vytvoření virtuálního počítače pomocí PowerShellu | Dokumentace Microsoftu"
description: "Rychle se naučíte, jak vytvářet virtuální počítače s Linuxem pomocí PowerShellu."
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
ms.openlocfilehash: adcf492d4c2d935c880167675a1ed97566156ec7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-linux-virtual-machine-with-powershell"></a><span data-ttu-id="ed39e-103">Vytvoření virtuálního počítače s Linuxem pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="ed39e-103">Create a Linux virtual machine with PowerShell</span></span>

<span data-ttu-id="ed39e-104">Modul Azure PowerShell slouží k vytváření a správě prostředků Azure z příkazového řádku PowerShellu nebo ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="ed39e-104">The Azure PowerShell module is used to create and manage Azure resources from the PowerShell command line or in scripts.</span></span> <span data-ttu-id="ed39e-105">Tento průvodce podrobně popisuje nasazení virtuálního počítače se serverem Ubuntu pomocí modulu Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ed39e-105">This guide details using the Azure PowerShell module to deploy a virtual machine running Ubuntu server.</span></span> <span data-ttu-id="ed39e-106">Po nasazení serveru se vytvoří připojení SSH a nainstaluje se webový server NGINX.</span><span class="sxs-lookup"><span data-stu-id="ed39e-106">Once the server is deployed, an SSH connection is created, and an NGINX webserver is installed.</span></span>

<span data-ttu-id="ed39e-107">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="ed39e-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

<span data-ttu-id="ed39e-108">Tento Rychlý start vyžaduje modul Azure PowerShell verze 3.6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ed39e-108">This quick start requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="ed39e-109">Verzi zjistíte spuštěním příkazu ` Get-Module -ListAvailable AzureRM`.</span><span class="sxs-lookup"><span data-stu-id="ed39e-109">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="ed39e-110">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="ed39e-110">If you need to install or upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

<span data-ttu-id="ed39e-111">Veřejný klíč SSH s názvem *id_rsa.pub* musí být nakonec uložen v adresáři *.ssh* vašeho profilu uživatele Windows.</span><span class="sxs-lookup"><span data-stu-id="ed39e-111">Finally, a public SSH key with the name *id_rsa.pub* needs to be stored in the *.ssh* directory of your Windows user profile.</span></span> <span data-ttu-id="ed39e-112">Podrobné informace o vytváření klíčů SSH pro Azure najdete v tématu popisujícím [vytvoření klíčů SSH pro Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ed39e-112">For detailed information on creating SSH keys for Azure, see [Create SSH keys for Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="log-in-to-azure"></a><span data-ttu-id="ed39e-113">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="ed39e-113">Log in to Azure</span></span>

<span data-ttu-id="ed39e-114">Přihlaste se k předplatnému Azure pomocí příkazu `Login-AzureRmAccount` a postupujte podle pokynů na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="ed39e-114">Log in to your Azure subscription with the `Login-AzureRmAccount` command and follow the on-screen directions.</span></span>

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a><span data-ttu-id="ed39e-115">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="ed39e-115">Create resource group</span></span>

<span data-ttu-id="ed39e-116">Vytvořte skupinu prostředků Azure pomocí příkazu [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="ed39e-116">Create an Azure resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="ed39e-117">Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="ed39e-117">A resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location eastus
```

## <a name="create-networking-resources"></a><span data-ttu-id="ed39e-118">Vytvoření síťových prostředků</span><span class="sxs-lookup"><span data-stu-id="ed39e-118">Create networking resources</span></span>

<span data-ttu-id="ed39e-119">Vytvořte virtuální síť, podsíť a veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="ed39e-119">Create a virtual network, subnet, and a public IP address.</span></span> <span data-ttu-id="ed39e-120">Tyto prostředky slouží k zajištění síťového připojení virtuálnímu počítači a k jeho připojení k internetu.</span><span class="sxs-lookup"><span data-stu-id="ed39e-120">These resources are used to provide network connectivity to the virtual machine and connect it to the internet.</span></span>

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

<span data-ttu-id="ed39e-121">Vytvořte skupinu zabezpečení sítě a pravidlo skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="ed39e-121">Create a network security group and a network security group rule.</span></span> <span data-ttu-id="ed39e-122">Skupina zabezpečení sítě zabezpečuje virtuální počítač pomocí příchozích a odchozích pravidel.</span><span class="sxs-lookup"><span data-stu-id="ed39e-122">The network security group secures the virtual machine using inbound and outbound rules.</span></span> <span data-ttu-id="ed39e-123">V tomto případě se vytvoří příchozí pravidlo pro port 22, které umožní příchozí připojení SSH.</span><span class="sxs-lookup"><span data-stu-id="ed39e-123">In this case, an inbound rule is created for port 22, which allows incoming SSH connections.</span></span> <span data-ttu-id="ed39e-124">Chceme také vytvořit příchozí pravidlo pro port 80, které umožní příchozí webový provoz.</span><span class="sxs-lookup"><span data-stu-id="ed39e-124">We also want to create an inbound rule for port 80, which allows incoming web traffic.</span></span>

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

<span data-ttu-id="ed39e-125">Vytvořte pro virtuální počítač síťovou kartu pomocí příkazu [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span><span class="sxs-lookup"><span data-stu-id="ed39e-125">Create a network card with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) for the virtual machine.</span></span> <span data-ttu-id="ed39e-126">Síťová karta připojuje virtuální počítač k podsíti, skupině zabezpečení sítě a veřejné IP adrese.</span><span class="sxs-lookup"><span data-stu-id="ed39e-126">The network card connects the virtual machine to a subnet, network security group, and public IP address.</span></span>

```powershell
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName myResourceGroup -Location eastus `
-SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-virtual-machine"></a><span data-ttu-id="ed39e-127">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="ed39e-127">Create virtual machine</span></span>

<span data-ttu-id="ed39e-128">Vytvořte konfiguraci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ed39e-128">Create a virtual machine configuration.</span></span> <span data-ttu-id="ed39e-129">Tato konfigurace zahrnuje nastavení, která se používají při nasazení virtuálního počítače, jako je image virtuálního počítače, jeho velikost a konfigurace ověřování.</span><span class="sxs-lookup"><span data-stu-id="ed39e-129">This configuration includes the settings that are used when deploying the virtual machine such as a virtual machine image, size, and authentication configuration.</span></span>

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

<span data-ttu-id="ed39e-130">Vytvořte virtuální počítač pomocí příkazu [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="ed39e-130">Create the virtual machine with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span>

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroup -Location eastus -VM $vmConfig
```

## <a name="connect-to-virtual-machine"></a><span data-ttu-id="ed39e-131">Připojení k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="ed39e-131">Connect to virtual machine</span></span>

<span data-ttu-id="ed39e-132">Po dokončení nasazení vytvořte připojení SSH k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="ed39e-132">After the deployment has completed, create an SSH connection with the virtual machine.</span></span>

<span data-ttu-id="ed39e-133">Použijte příkaz [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress), který vrátí veřejnou IP adresu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ed39e-133">Use the [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) command to return the public IP address of the virtual machine.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

<span data-ttu-id="ed39e-134">Ze systému s nainstalovaným SSH se pomocí následujícího příkazu připojte k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="ed39e-134">From a system with SSH installed, used the following command to connect to the virtual machine.</span></span> <span data-ttu-id="ed39e-135">Pokud pracujete v systému Windows, můžete k vytvoření připojení použít [PuTTy](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-ssh-from-windows?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-private-key-for-putty).</span><span class="sxs-lookup"><span data-stu-id="ed39e-135">If working on Windows, [Putty](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-ssh-from-windows?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-private-key-for-putty) can be used to create the connection.</span></span> 

```bash 
ssh <Public IP Address>
```

<span data-ttu-id="ed39e-136">Po zobrazení výzvy zadejte uživatelské jméno *azureuser*.</span><span class="sxs-lookup"><span data-stu-id="ed39e-136">When prompted, the login user name is *azureuser*.</span></span> <span data-ttu-id="ed39e-137">Pokud jste při vytváření klíčů SSH zadali heslo, musíte zadat i to.</span><span class="sxs-lookup"><span data-stu-id="ed39e-137">If a passphrase was entered when creating SSH keys, you need to enter this as well.</span></span>


## <a name="install-nginx"></a><span data-ttu-id="ed39e-138">Instalace serveru NGINX</span><span class="sxs-lookup"><span data-stu-id="ed39e-138">Install NGINX</span></span>

<span data-ttu-id="ed39e-139">Pomocí následujícího skriptu bash provedete aktualizaci zdrojů balíčku a nainstalujete nejnovější balíček NGINX.</span><span class="sxs-lookup"><span data-stu-id="ed39e-139">Use the following bash script to update package sources and install the latest NGINX package.</span></span> 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-ngix-welcome-page"></a><span data-ttu-id="ed39e-140">Zobrazení úvodní stránky serveru NGINX</span><span class="sxs-lookup"><span data-stu-id="ed39e-140">View the NGIX welcome page</span></span>

<span data-ttu-id="ed39e-141">S nainstalovaným serverem NGINX na virtuálním počítači a nyní otevřeným portem 80 z internetu můžete použít libovolný webový prohlížeč a zobrazit výchozí úvodní stránku serveru NGINX.</span><span class="sxs-lookup"><span data-stu-id="ed39e-141">With NGINX installed and port 80 now open on your VM from the Internet - you can use a web browser of your choice to view the default NGINX welcome page.</span></span> <span data-ttu-id="ed39e-142">Nezapomeňte pro návštěvu výchozí stránky použít veřejnou IP adresu popsanou výše.</span><span class="sxs-lookup"><span data-stu-id="ed39e-142">Be sure to use the public IP address you documented above to visit the default page.</span></span> 

![Výchozí web NGINX](./media/quick-create-cli/nginx.png) 

## <a name="clean-up-resources"></a><span data-ttu-id="ed39e-144">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="ed39e-144">Clean up resources</span></span>

<span data-ttu-id="ed39e-145">Pokud už je nepotřebujete, můžete k odebrání skupiny prostředků, virtuálního počítače a všech souvisejících prostředků použít příkaz [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="ed39e-145">When no longer needed, you can use the [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="ed39e-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ed39e-146">Next steps</span></span>

<span data-ttu-id="ed39e-147">V tomto Rychlém startu jste nasadili jednoduchý virtuální počítač a pravidlo skupiny zabezpečení sítě a nainstalovali jste webový server.</span><span class="sxs-lookup"><span data-stu-id="ed39e-147">In this quick start, you’ve deployed a simple virtual machine, a network security group rule, and installed a web server.</span></span> <span data-ttu-id="ed39e-148">Další informace o virtuálních počítačích Azure najdete v kurzu pro virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="ed39e-148">To learn more about Azure virtual machines, continue to the tutorial for Linux VMs.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ed39e-149">Kurzy pro virtuální počítače Azure s Linuxem</span><span class="sxs-lookup"><span data-stu-id="ed39e-149">Azure Linux virtual machine tutorials</span></span>](./tutorial-manage-vm.md)
