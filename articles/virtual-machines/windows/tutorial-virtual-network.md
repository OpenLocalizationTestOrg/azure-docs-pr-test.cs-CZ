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
# <a name="manage-azure-virtual-networks-and-windows-virtual-machines-with-azure-powershell"></a><span data-ttu-id="f1424-103">Správa virtuálních sítí Azure a virtuální počítače s Windows pomocí Azure Powershellu</span><span class="sxs-lookup"><span data-stu-id="f1424-103">Manage Azure Virtual Networks and Windows Virtual Machines with Azure PowerShell</span></span>

<span data-ttu-id="f1424-104">Virtuální počítače Azure pomocí Azure sítě pro vnitřní a vnější síťovou komunikaci.</span><span class="sxs-lookup"><span data-stu-id="f1424-104">Azure virtual machines use Azure networking for internal and external network communication.</span></span> <span data-ttu-id="f1424-105">V tomto kurzu vytvoříte více virtuálních počítačů (VM) ve virtuální síti a konfigurace připojení k síti mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="f1424-105">In this tutorial, you create multiple virtual machines (VMs) in a virtual network and configure network connectivity between them.</span></span> <span data-ttu-id="f1424-106">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="f1424-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f1424-107">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="f1424-107">Create a virtual network</span></span>
> * <span data-ttu-id="f1424-108">Vytvoření podsítě virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="f1424-108">Create virtual network subnets</span></span>
> * <span data-ttu-id="f1424-109">Ovládací prvek síťový provoz se skupinami zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="f1424-109">Control network traffic with Network Security Groups</span></span>
> * <span data-ttu-id="f1424-110">Zobrazení pravidla pro provoz v akci</span><span class="sxs-lookup"><span data-stu-id="f1424-110">View traffic rules in action</span></span>

<span data-ttu-id="f1424-111">Tento kurz vyžaduje modul Azure PowerShell verze 3.6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f1424-111">This tutorial requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="f1424-112">Verzi zjistíte spuštěním příkazu ` Get-Module -ListAvailable AzureRM`.</span><span class="sxs-lookup"><span data-stu-id="f1424-112">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="f1424-113">Pokud je třeba upgradovat, přečtěte si téma [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="f1424-113">If you need to upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="create-vnet"></a><span data-ttu-id="f1424-114">Vytvoření sítě VNet</span><span class="sxs-lookup"><span data-stu-id="f1424-114">Create VNet</span></span>

<span data-ttu-id="f1424-115">Virtuální síť je reprezentace vlastní sítě v cloudu.</span><span class="sxs-lookup"><span data-stu-id="f1424-115">A VNet is a representation of your own network in the cloud.</span></span> <span data-ttu-id="f1424-116">Virtuální síť je to logická izolace cloudu Azure vyhrazeného pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="f1424-116">A VNet is a logical isolation of the Azure cloud dedicated to your subscription.</span></span> <span data-ttu-id="f1424-117">V rámci virtuální sítě zjistíte, podsítě, pravidla pro připojení k těchto podsítí a připojení z virtuálních počítačů k podsítím.</span><span class="sxs-lookup"><span data-stu-id="f1424-117">Within a VNet, you find subnets, rules for connectivity to those subnets, and connections from the VMs to the subnets.</span></span>

<span data-ttu-id="f1424-118">Před vytvořením veškeré prostředky Azure, je nutné vytvořit skupinu prostředků s [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="f1424-118">Before you can create any other Azure resources, you need to create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="f1424-119">Následující příklad vytvoří skupinu prostředků s názvem *myRGNetwork* v *EastUS* umístění:</span><span class="sxs-lookup"><span data-stu-id="f1424-119">The following example creates a resource group named *myRGNetwork* in the *EastUS* location:</span></span>

```powershell
New-AzureRmResourceGroup -ResourceGroupName myRGNetwork -Location EastUS
```

<span data-ttu-id="f1424-120">Podsíť je prostředkem podřízené virtuální sítě, a pomáhá definovat segmenty adresní prostory v rámci blok CIDR pomocí předpony IP adres.</span><span class="sxs-lookup"><span data-stu-id="f1424-120">A subnet is a child resource of a VNet, and helps define segments of address spaces within a CIDR block, using IP address prefixes.</span></span> <span data-ttu-id="f1424-121">Síťové adaptéry můžete přidat do podsítí a připojení k virtuálním počítačům, poskytuje připojení pro různé úlohy.</span><span class="sxs-lookup"><span data-stu-id="f1424-121">NICs can be added to subnets, and connected to VMs, providing connectivity for various workloads.</span></span>

<span data-ttu-id="f1424-122">Vytvořte podsíť s [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="f1424-122">Create a subnet with [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):</span></span>

```powershell
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myFrontendSubnet `
  -AddressPrefix 10.0.0.0/24
```

<span data-ttu-id="f1424-123">Vytvoření virtuální sítě s názvem *myVNet* pomocí *myFrontendSubnet* s [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="f1424-123">Create a VNET named *myVNet* using *myFrontendSubnet* with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $frontendSubnet
```

## <a name="create-front-end-vm"></a><span data-ttu-id="f1424-124">Vytvoření virtuálního počítače s front-endu</span><span class="sxs-lookup"><span data-stu-id="f1424-124">Create front-end VM</span></span>

<span data-ttu-id="f1424-125">Pro virtuální počítač pro komunikaci ve virtuální síti musí být virtuální síťové rozhraní (NIC).</span><span class="sxs-lookup"><span data-stu-id="f1424-125">For a VM to communicate in a VNet, it needs a virtual network interface (NIC).</span></span> <span data-ttu-id="f1424-126">*MyFrontendVM* přístupná z Internetu, takže je také nutné veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="f1424-126">The *myFrontendVM* is accessed from the internet, so it also needs a public IP address.</span></span> 

<span data-ttu-id="f1424-127">Vytvoření veřejné IP adresy s [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):</span><span class="sxs-lookup"><span data-stu-id="f1424-127">Create a public IP address with [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):</span></span>

```powershell
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIPAddress
```

<span data-ttu-id="f1424-128">Vytvořit síťový adaptér s [nové AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="f1424-128">Create a NIC with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):</span></span>


```powershell
$frontendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontendNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

<span data-ttu-id="f1424-129">Nastavte uživatelské jméno a heslo potřebné pro správce účtu na virtuálním počítači s [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="f1424-129">Set the username and password needed for the administrator account on the VM with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="f1424-130">Vytvoření virtuálních počítačů s [nové AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk), [přidat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface), a [nové AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="f1424-130">Create the VMs with [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk), [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface), and [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> 

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

## <a name="install-web-server"></a><span data-ttu-id="f1424-131">Instalace webového serveru</span><span class="sxs-lookup"><span data-stu-id="f1424-131">Install web server</span></span>

<span data-ttu-id="f1424-132">Nainstalovat službu IIS na *myFrontendVM* pomocí relaci vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="f1424-132">You can install IIS on *myFrontendVM* by using a remote desktop session.</span></span> <span data-ttu-id="f1424-133">Budete muset získat veřejnou IP adresu virtuálního počítače k přístupu.</span><span class="sxs-lookup"><span data-stu-id="f1424-133">You need to get the public IP address of the VM to access it.</span></span>

<span data-ttu-id="f1424-134">Můžete získat veřejnou IP adresu *myFrontendVM* s [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="f1424-134">You can get the public IP address of *myFrontendVM* with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="f1424-135">Následující příklad, získá IP adresu pro *myPublicIPAddress* vytvořili dříve:</span><span class="sxs-lookup"><span data-stu-id="f1424-135">The following example obtains the IP address for *myPublicIPAddress* created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName myRGNetwork `
    -Name myPublicIPAddress | select IpAddress
```

<span data-ttu-id="f1424-136">Poznamenejte si tuto IP adresu, můžete ji použít v budoucnu krocích.</span><span class="sxs-lookup"><span data-stu-id="f1424-136">Take note of this IP Address so you can use it in future steps.</span></span>

<span data-ttu-id="f1424-137">Použijte následující příkaz Vytvořit relaci vzdálené plochy s *myFrontendVM*.</span><span class="sxs-lookup"><span data-stu-id="f1424-137">Use the following command to create a remote desktop session with *myFrontendVM*.</span></span> <span data-ttu-id="f1424-138">Nahraďte  *<publicIPAddress>*  adresy, která jste si předtím poznamenali.</span><span class="sxs-lookup"><span data-stu-id="f1424-138">Replace *<publicIPAddress>* with the address that you previously recorded.</span></span> <span data-ttu-id="f1424-139">Po zobrazení výzvy zadejte přihlašovací údaje použité při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f1424-139">When prompted, enter the credentials used when you created the VM.</span></span>

```
mstsc /v:<publicIpAddress>
``` 

<span data-ttu-id="f1424-140">Teď, když jste přihlášení k *myFrontendVM*, jeden řádek prostředí PowerShell můžete použít k instalaci IIS a aktivace pravidla místní brány firewall pro povolení webový provoz.</span><span class="sxs-lookup"><span data-stu-id="f1424-140">Now that you have logged in to *myFrontendVM*, you can use a single line of PowerShell to install IIS and enable the local firewall rule to allow web traffic.</span></span> <span data-ttu-id="f1424-141">Otevřete příkazový řádek PowerShellu a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f1424-141">Open a PowerShell prompt and run the following command:</span></span>

<span data-ttu-id="f1424-142">Použití [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/servermanager/install-windowsfeature) ke spuštění rozšíření vlastních skriptů, který instaluje webový server služby IIS:</span><span class="sxs-lookup"><span data-stu-id="f1424-142">Use [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/servermanager/install-windowsfeature) to run the custom script extension that installs the IIS webserver:</span></span>

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

<span data-ttu-id="f1424-143">Teď můžete použít veřejnou IP adresu a přejděte do virtuálního počítače najdete v části webu služby IIS.</span><span class="sxs-lookup"><span data-stu-id="f1424-143">Now you can use the public IP address to browse to the VM to see the IIS site.</span></span>

![Výchozí web služby IIS](./media/tutorial-virtual-network/iis.png)

## <a name="manage-internal-traffic"></a><span data-ttu-id="f1424-145">Spravovat interní provoz</span><span class="sxs-lookup"><span data-stu-id="f1424-145">Manage internal traffic</span></span>

<span data-ttu-id="f1424-146">Skupina zabezpečení sítě (NSG) obsahuje seznam pravidel zabezpečení, která povolují nebo odpírají síťový provoz na prostředky, které jsou připojené k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="f1424-146">A network security group (NSG) contains a list of security rules that allow or deny network traffic to resources connected to a VNet.</span></span> <span data-ttu-id="f1424-147">Skupiny Nsg můžou být přidružena k podsítě nebo jednotlivé síťové adaptéry připojené k virtuálním počítačům.</span><span class="sxs-lookup"><span data-stu-id="f1424-147">NSGs can be associated to subnets or individual NICs attached to VMs.</span></span> <span data-ttu-id="f1424-148">Otevírání nebo při zavření přístup k virtuálním počítačům prostřednictvím portů se provádí pomocí pravidla NSG.</span><span class="sxs-lookup"><span data-stu-id="f1424-148">Opening or closing access to VMs through ports is done using NSG rules.</span></span> <span data-ttu-id="f1424-149">Při vytváření *myFrontendVM*, příchozí port 3389 se automaticky otevře pro připojení RDP.</span><span class="sxs-lookup"><span data-stu-id="f1424-149">When you created *myFrontendVM*, inbound port 3389 was automatically opened for RDP connectivity.</span></span>

<span data-ttu-id="f1424-150">Interní komunikaci virtuálních počítačů můžete konfigurovat pomocí skupinu NSG.</span><span class="sxs-lookup"><span data-stu-id="f1424-150">Internal communication of VMs can be configured using an NSG.</span></span> <span data-ttu-id="f1424-151">V této části se dozvíte, jak vytvořit další podsítě v síti a přiřadit skupinu NSG k němu povolit připojení z *myFrontendVM* k *myBackendVM* na portu 1433.</span><span class="sxs-lookup"><span data-stu-id="f1424-151">In this section, you learn how to create an additional subnet in the network and assign an NSG to it to allow a connection from *myFrontendVM* to *myBackendVM* on port 1433.</span></span> <span data-ttu-id="f1424-152">Podsíť je pak přiřazené k virtuálnímu počítači při jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="f1424-152">The subnet is then assigned to the VM when it is created.</span></span>

<span data-ttu-id="f1424-153">Můžete omezit interní provoz *myBackendVM* z jenom *myFrontendVM* tak, že vytvoříte skupinu NSG pro podsíť back-end.</span><span class="sxs-lookup"><span data-stu-id="f1424-153">You can limit internal traffic to *myBackendVM* from only *myFrontendVM* by creating an NSG for the back-end subnet.</span></span> <span data-ttu-id="f1424-154">Následující příklad vytvoří pravidlo NSG s názvem *myBackendNSGRule* s [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig):</span><span class="sxs-lookup"><span data-stu-id="f1424-154">The following example creates an NSG rule named *myBackendNSGRule* with [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig):</span></span>

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

<span data-ttu-id="f1424-155">Přidat skupinu zabezpečení sítě s názvem *myBackendNSG* s [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):</span><span class="sxs-lookup"><span data-stu-id="f1424-155">Add a network security group named *myBackendNSG* with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):</span></span>

```powershell
$nsgBackend = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNSG `
  -SecurityRules $nsgBackendRule
```
## <a name="add-back-end-subnet"></a><span data-ttu-id="f1424-156">Přidat podsíť back-end</span><span class="sxs-lookup"><span data-stu-id="f1424-156">Add back-end subnet</span></span>

<span data-ttu-id="f1424-157">Přidat *myBackEndSubnet* k *myVNet* s [přidat AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="f1424-157">Add *myBackEndSubnet* to *myVNet* with [Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig):</span></span>

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

## <a name="create-back-end-vm"></a><span data-ttu-id="f1424-158">Vytvoření virtuálního počítače s back-end</span><span class="sxs-lookup"><span data-stu-id="f1424-158">Create back-end VM</span></span>

<span data-ttu-id="f1424-159">Nejjednodušší způsob, jak vytvořit virtuální počítač back-end je pomocí bitové kopie systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f1424-159">The easiest way to create the back-end VM is by using a SQL Server image.</span></span> <span data-ttu-id="f1424-160">V tomto kurzu pouze vytvoří virtuální počítač s databázovým serverem, ale neposkytuje informace o přístupu k databázi.</span><span class="sxs-lookup"><span data-stu-id="f1424-160">This tutorial only creates the VM with the database server, but doesn't provide information about accessing the database.</span></span>

<span data-ttu-id="f1424-161">Vytvoření *myBackendNic*:</span><span class="sxs-lookup"><span data-stu-id="f1424-161">Create *myBackendNic*:</span></span>

```powershell
$backendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNic `
  -SubnetId $vnet.Subnets[1].Id
```

<span data-ttu-id="f1424-162">Nastavte uživatelské jméno a heslo pro účet správce ve virtuálním počítači s Get-Credential potřebné:</span><span class="sxs-lookup"><span data-stu-id="f1424-162">Set the username and password needed for the administrator account on the VM with Get-Credential:</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="f1424-163">Vytvoření *myBackendVM*:</span><span class="sxs-lookup"><span data-stu-id="f1424-163">Create *myBackendVM*:</span></span>

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

<span data-ttu-id="f1424-164">Obrázek, který je použit nainstalován server SQL, ale není použit v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="f1424-164">The image that is used has SQL Server installed, but is not used in this tutorial.</span></span> <span data-ttu-id="f1424-165">Je zahrnutá ukázat vám, jak můžete nakonfigurovat virtuální počítač pro zpracování webových přenosů a virtuální počítač pro správu databáze.</span><span class="sxs-lookup"><span data-stu-id="f1424-165">It is included to show you how you can configure a VM to handle web traffic and a VM to handle database management.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f1424-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f1424-166">Next steps</span></span>

<span data-ttu-id="f1424-167">V tomto kurzu jste vytvořili a zabezpečené sítě v souvislosti s virtuálními počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="f1424-167">In this tutorial, you created and secured Azure networks as related to virtual machines.</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="f1424-168">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="f1424-168">Create a virtual network</span></span>
> * <span data-ttu-id="f1424-169">Vytvoření podsítě virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="f1424-169">Create virtual network subnets</span></span>
> * <span data-ttu-id="f1424-170">Ovládací prvek síťový provoz se skupinami zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="f1424-170">Control network traffic with Network Security Groups</span></span>
> * <span data-ttu-id="f1424-171">Zobrazení pravidla pro provoz v akci</span><span class="sxs-lookup"><span data-stu-id="f1424-171">View traffic rules in action</span></span>

<span data-ttu-id="f1424-172">Přechodu na v dalším kurzu se dozvíte o monitorování zabezpečení dat na virtuální počítače pomocí zálohování Azure.</span><span class="sxs-lookup"><span data-stu-id="f1424-172">Advance to the next tutorial to learn about monitoring securing data on virtual machines using Azure backup.</span></span> <span data-ttu-id="f1424-173">.</span><span class="sxs-lookup"><span data-stu-id="f1424-173">.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f1424-174">Zálohovat virtuální počítače s Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="f1424-174">Back up Windows virtual machines in Azure</span></span>](./tutorial-backup-vms.md)
