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
# <a name="create-a-windows-virtual-machine-with-powershell"></a><span data-ttu-id="db586-103">Vytvoření virtuálního počítače s Windows pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="db586-103">Create a Windows virtual machine with PowerShell</span></span>

<span data-ttu-id="db586-104">modul Azure PowerShell Hello je použité toocreate a spravovat prostředky Azure z příkazového řádku Powershellu hello nebo ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="db586-104">hello Azure PowerShell module is used toocreate and manage Azure resources from hello PowerShell command line or in scripts.</span></span> <span data-ttu-id="db586-105">Tato příručka údaje pomocí prostředí PowerShell toocreate a Azure virtuální počítač se systémem Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="db586-105">This guide details using PowerShell toocreate and Azure virtual machine running Windows Server 2016.</span></span> <span data-ttu-id="db586-106">Po dokončení nasazení jsme připojení toohello serveru a instalace služby IIS.</span><span class="sxs-lookup"><span data-stu-id="db586-106">Once deployment is complete, we connect toohello server and install IIS.</span></span>  

<span data-ttu-id="db586-107">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="db586-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

<span data-ttu-id="db586-108">Tento úvodní vyžaduje hello prostředí Azure PowerShell verze modulu 3,6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="db586-108">This quick start requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="db586-109">Spustit ` Get-Module -ListAvailable AzureRM` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="db586-109">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="db586-110">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="db586-110">If you need tooinstall or upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="db586-111">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="db586-111">Log in tooAzure</span></span>

<span data-ttu-id="db586-112">Přihlaste se tooyour předplatné s hello `Login-AzureRmAccount` příkazů a postupujte podle hello na obrazovce pokynů.</span><span class="sxs-lookup"><span data-stu-id="db586-112">Log in tooyour Azure subscription with hello `Login-AzureRmAccount` command and follow hello on-screen directions.</span></span>

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a><span data-ttu-id="db586-113">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="db586-113">Create resource group</span></span>

<span data-ttu-id="db586-114">Vytvořte skupinu prostředků Azure pomocí příkazu [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="db586-114">Create an Azure resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="db586-115">Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="db586-115">A resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-networking-resources"></a><span data-ttu-id="db586-116">Vytvoření síťových prostředků</span><span class="sxs-lookup"><span data-stu-id="db586-116">Create networking resources</span></span>

### <a name="create-a-virtual-network-subnet-and-a-public-ip-address"></a><span data-ttu-id="db586-117">Vytvořte virtuální síť, podsíť a veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="db586-117">Create a virtual network, subnet, and a public IP address.</span></span> 
<span data-ttu-id="db586-118">Tyto prostředky jsou použité tooprovide síťové připojení toohello virtuální počítač a připojte ho toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="db586-118">These resources are used tooprovide network connectivity toohello virtual machine and connect it toohello internet.</span></span>

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

### <a name="create-a-network-security-group-and-a-network-security-group-rule"></a><span data-ttu-id="db586-119">Vytvořte skupinu zabezpečení sítě a pravidlo skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="db586-119">Create a network security group and a network security group rule.</span></span> 
<span data-ttu-id="db586-120">Skupina zabezpečení sítě Hello zabezpečuje hello virtuálního počítače pomocí příchozí a odchozí pravidla.</span><span class="sxs-lookup"><span data-stu-id="db586-120">hello network security group secures hello virtual machine using inbound and outbound rules.</span></span> <span data-ttu-id="db586-121">V tomto případě se vytvoří příchozí pravidlo pro port 3389, které umožní příchozí připojení ke vzdálené ploše.</span><span class="sxs-lookup"><span data-stu-id="db586-121">In this case, an inbound rule is created for port 3389, which allows incoming remote desktop connections.</span></span> <span data-ttu-id="db586-122">Chceme také toocreate příchozí pravidlo pro port 80, která umožní příchozí webový provoz.</span><span class="sxs-lookup"><span data-stu-id="db586-122">We also want toocreate an inbound rule for port 80, which allows incoming web traffic.</span></span>

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

### <a name="create-a-network-card-for-hello-virtual-machine"></a><span data-ttu-id="db586-123">Vytvořte síťové karty pro virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="db586-123">Create a network card for hello virtual machine.</span></span> 
<span data-ttu-id="db586-124">Vytvoření síťové karty s [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="db586-124">Create a network card with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) for hello virtual machine.</span></span> <span data-ttu-id="db586-125">Hello síťová karta připojí podsíť tooa hello virtuální počítač, skupina zabezpečení sítě a veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="db586-125">hello network card connects hello virtual machine tooa subnet, network security group, and public IP address.</span></span>

```powershell
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName myResourceGroup -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-virtual-machine"></a><span data-ttu-id="db586-126">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="db586-126">Create virtual machine</span></span>

<span data-ttu-id="db586-127">Vytvořte konfiguraci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="db586-127">Create a virtual machine configuration.</span></span> <span data-ttu-id="db586-128">Tato konfigurace zahrnuje hello nastavení, které se použijí při nasazení virtuálního počítače hello třeba konfiguraci virtuálního počítače bitové kopie, velikost a ověřování.</span><span class="sxs-lookup"><span data-stu-id="db586-128">This configuration includes hello settings that are used when deploying hello virtual machine such as a virtual machine image, size, and authentication configuration.</span></span> <span data-ttu-id="db586-129">Při spuštění tohoto kroku se zobrazí výzva k zadání přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="db586-129">When running this step, you are prompted for credentials.</span></span> <span data-ttu-id="db586-130">Hello hodnoty, které zadáte, jsou nakonfigurovány jako hello uživatelské jméno a heslo pro virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="db586-130">hello values that you enter are configured as hello user name and password for hello virtual machine.</span></span>

```powershell
# Define a credential object
$cred = Get-Credential

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS2 | `
    Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
    Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer `
    -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nic.Id
```

<span data-ttu-id="db586-131">Vytvoření virtuálního počítače hello s [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="db586-131">Create hello virtual machine with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span>

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroup -Location EastUS -VM $vmConfig
```

## <a name="connect-toovirtual-machine"></a><span data-ttu-id="db586-132">Připojte počítač toovirtual</span><span class="sxs-lookup"><span data-stu-id="db586-132">Connect toovirtual machine</span></span>

<span data-ttu-id="db586-133">Po dokončení nasazení hello vytvořte připojení ke vzdálené ploše se hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="db586-133">After hello deployment has completed, create a remote desktop connection with hello virtual machine.</span></span>

<span data-ttu-id="db586-134">Použití hello [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) příkaz tooreturn hello veřejnou IP adresu hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="db586-134">Use hello [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) command tooreturn hello public IP address of hello virtual machine.</span></span> <span data-ttu-id="db586-135">Poznamenejte si tuto IP adresu, tooit můžete propojit s váš prohlížeč tootest připojení k webu v příštím kroku.</span><span class="sxs-lookup"><span data-stu-id="db586-135">Take note of this IP Address so you can connect tooit with your browser tootest web connectivity in a future step.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

<span data-ttu-id="db586-136">Použití hello následující příkaz toocreate a relace vzdálené plochy s hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="db586-136">Use hello following command toocreate a remote desktop session with hello virtual machine.</span></span> <span data-ttu-id="db586-137">Nahraďte IP adresu hello hello *publicIPAddress* virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="db586-137">Replace hello IP address with hello *publicIPAddress* of your virtual machine.</span></span> <span data-ttu-id="db586-138">Po zobrazení výzvy zadejte přihlašovací údaje hello použité při vytváření hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="db586-138">When prompted, enter hello credentials used when creating hello virtual machine.</span></span>

```bash 
mstsc /v:<publicIpAddress>
```

## <a name="install-iis-via-powershell"></a><span data-ttu-id="db586-139">Instalace služby IIS pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="db586-139">Install IIS via PowerShell</span></span>

<span data-ttu-id="db586-140">Teď, když jste byli přihlášeni toohello virtuálního počítače Azure, můžete použít jeden řádek tooinstall prostředí PowerShell služby IIS a povolit hello místní brány firewall pravidla tooallow webový provoz.</span><span class="sxs-lookup"><span data-stu-id="db586-140">Now that you have logged in toohello Azure VM, you can use a single line of PowerShell tooinstall IIS and enable hello local firewall rule tooallow web traffic.</span></span> <span data-ttu-id="db586-141">Otevřete příkazovém řádku prostředí PowerShell a spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="db586-141">Open a PowerShell prompt and run hello following command:</span></span>

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-hello-iis-welcome-page"></a><span data-ttu-id="db586-142">Zobrazení hello úvodní stránka služby IIS</span><span class="sxs-lookup"><span data-stu-id="db586-142">View hello IIS welcome page</span></span>

<span data-ttu-id="db586-143">S nainstalovanou službu IIS a port 80 nyní otevřete na vašem virtuálním počítači z hello Internetu můžete použít webový prohlížeč choice tooview hello výchozí IIS uvítací stránky.</span><span class="sxs-lookup"><span data-stu-id="db586-143">With IIS installed and port 80 now open on your VM from hello Internet, you can use a web browser of your choice tooview hello default IIS welcome page.</span></span> <span data-ttu-id="db586-144">Se, zda text hello toouse *publicIpAddress* popsané výše toovisit hello výchozí stránky.</span><span class="sxs-lookup"><span data-stu-id="db586-144">Be sure toouse hello *publicIpAddress* you documented above toovisit hello default page.</span></span> 

![Výchozí web služby IIS](./media/quick-create-powershell/default-iis-website.png) 

## <a name="clean-up-resources"></a><span data-ttu-id="db586-146">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="db586-146">Clean up resources</span></span>

<span data-ttu-id="db586-147">Pokud již nepotřebujete, můžete použít hello [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) příkaz skupiny prostředků hello tooremove, virtuálních počítačů a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="db586-147">When no longer needed, you can use hello [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="db586-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="db586-148">Next steps</span></span>

<span data-ttu-id="db586-149">V tomto Rychlém startu jste nasadili jednoduchý virtuální počítač a pravidlo skupiny zabezpečení sítě a nainstalovali jste webový server.</span><span class="sxs-lookup"><span data-stu-id="db586-149">In this quick start, you’ve deployed a simple virtual machine, a network security group rule, and installed a web server.</span></span> <span data-ttu-id="db586-150">toolearn Další informace o virtuálních počítačích Azure, pokračovat v kurzu toohello pro virtuální počítače Windows.</span><span class="sxs-lookup"><span data-stu-id="db586-150">toolearn more about Azure virtual machines, continue toohello tutorial for Windows VMs.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="db586-151">Kurzy pro virtuální počítače Azure s Windows</span><span class="sxs-lookup"><span data-stu-id="db586-151">Azure Windows virtual machine tutorials</span></span>](./tutorial-manage-vm.md)
