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
# <a name="manage-azure-virtual-networks-and-windows-virtual-machines-with-azure-powershell"></a><span data-ttu-id="b6bd1-103">Správa virtuálních sítí Azure a virtuální počítače s Windows pomocí Azure Powershellu</span><span class="sxs-lookup"><span data-stu-id="b6bd1-103">Manage Azure Virtual Networks and Windows Virtual Machines with Azure PowerShell</span></span>

<span data-ttu-id="b6bd1-104">Virtuální počítače Azure pomocí Azure sítě pro vnitřní a vnější síťovou komunikaci.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-104">Azure virtual machines use Azure networking for internal and external network communication.</span></span> <span data-ttu-id="b6bd1-105">V tomto kurzu vytvoříte více virtuálních počítačů (VM) ve virtuální síti a konfigurace připojení k síti mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-105">In this tutorial, you create multiple virtual machines (VMs) in a virtual network and configure network connectivity between them.</span></span> <span data-ttu-id="b6bd1-106">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="b6bd1-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b6bd1-107">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="b6bd1-107">Create a virtual network</span></span>
> * <span data-ttu-id="b6bd1-108">Vytvoření podsítě virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="b6bd1-108">Create virtual network subnets</span></span>
> * <span data-ttu-id="b6bd1-109">Ovládací prvek síťový provoz se skupinami zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="b6bd1-109">Control network traffic with Network Security Groups</span></span>
> * <span data-ttu-id="b6bd1-110">Zobrazení pravidla pro provoz v akci</span><span class="sxs-lookup"><span data-stu-id="b6bd1-110">View traffic rules in action</span></span>

<span data-ttu-id="b6bd1-111">Tento kurz vyžaduje hello prostředí Azure PowerShell verze modulu 3,6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-111">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="b6bd1-112">Spustit ` Get-Module -ListAvailable AzureRM` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-112">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="b6bd1-113">Pokud potřebujete tooupgrade, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="b6bd1-113">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="create-vnet"></a><span data-ttu-id="b6bd1-114">Vytvoření sítě VNet</span><span class="sxs-lookup"><span data-stu-id="b6bd1-114">Create VNet</span></span>

<span data-ttu-id="b6bd1-115">Virtuální síť je reprezentace vlastní sítě v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-115">A VNet is a representation of your own network in hello cloud.</span></span> <span data-ttu-id="b6bd1-116">Virtuální síť je to logická izolace cloudu Azure vyhrazeného tooyour předplatné hello.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-116">A VNet is a logical isolation of hello Azure cloud dedicated tooyour subscription.</span></span> <span data-ttu-id="b6bd1-117">V rámci virtuální sítě zjistíte, podsítě, pravidla pro podsítě toothose připojení a připojení z podsítě toohello hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-117">Within a VNet, you find subnets, rules for connectivity toothose subnets, and connections from hello VMs toohello subnets.</span></span>

<span data-ttu-id="b6bd1-118">Před vytvořením veškeré prostředky Azure, je nutné toocreate skupinu prostředků s [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="b6bd1-118">Before you can create any other Azure resources, you need toocreate a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="b6bd1-119">Hello následující příklad vytvoří skupinu prostředků s názvem *myRGNetwork* v hello *EastUS* umístění:</span><span class="sxs-lookup"><span data-stu-id="b6bd1-119">hello following example creates a resource group named *myRGNetwork* in hello *EastUS* location:</span></span>

```powershell
New-AzureRmResourceGroup -ResourceGroupName myRGNetwork -Location EastUS
```

<span data-ttu-id="b6bd1-120">Podsíť je prostředkem podřízené virtuální sítě, a pomáhá definovat segmenty adresní prostory v rámci blok CIDR pomocí předpony IP adres.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-120">A subnet is a child resource of a VNet, and helps define segments of address spaces within a CIDR block, using IP address prefixes.</span></span> <span data-ttu-id="b6bd1-121">Síťové adaptéry lze přidat toosubnets a připojené tooVMs, poskytuje připojení pro různé úlohy.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-121">NICs can be added toosubnets, and connected tooVMs, providing connectivity for various workloads.</span></span>

<span data-ttu-id="b6bd1-122">Vytvořte podsíť s [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="b6bd1-122">Create a subnet with [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):</span></span>

```powershell
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myFrontendSubnet `
  -AddressPrefix 10.0.0.0/24
```

<span data-ttu-id="b6bd1-123">Vytvoření virtuální sítě s názvem *myVNet* pomocí *myFrontendSubnet* s [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="b6bd1-123">Create a VNET named *myVNet* using *myFrontendSubnet* with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $frontendSubnet
```

## <a name="create-front-end-vm"></a><span data-ttu-id="b6bd1-124">Vytvoření virtuálního počítače s front-endu</span><span class="sxs-lookup"><span data-stu-id="b6bd1-124">Create front-end VM</span></span>

<span data-ttu-id="b6bd1-125">Pro virtuální počítač toocommunicate ve virtuální síti musí být virtuální síťové rozhraní (NIC).</span><span class="sxs-lookup"><span data-stu-id="b6bd1-125">For a VM toocommunicate in a VNet, it needs a virtual network interface (NIC).</span></span> <span data-ttu-id="b6bd1-126">Hello *myFrontendVM* je k němu přistupovat z hello Internetu, takže je také nutné veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-126">hello *myFrontendVM* is accessed from hello internet, so it also needs a public IP address.</span></span> 

<span data-ttu-id="b6bd1-127">Vytvoření veřejné IP adresy s [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):</span><span class="sxs-lookup"><span data-stu-id="b6bd1-127">Create a public IP address with [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):</span></span>

```powershell
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIPAddress
```

<span data-ttu-id="b6bd1-128">Vytvořit síťový adaptér s [nové AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="b6bd1-128">Create a NIC with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):</span></span>


```powershell
$frontendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontendNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

<span data-ttu-id="b6bd1-129">Nastavte hello uživatelské jméno a heslo, které potřebuje pro účet správce hello na hello virtuálních počítačů s [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="b6bd1-129">Set hello username and password needed for hello administrator account on hello VM with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="b6bd1-130">Vytvoření hello virtuálních počítačů s [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk), [Přidat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface), a [nové AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="b6bd1-130">Create hello VMs with [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk), [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface), and [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> 

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

## <a name="install-web-server"></a><span data-ttu-id="b6bd1-131">Instalace webového serveru</span><span class="sxs-lookup"><span data-stu-id="b6bd1-131">Install web server</span></span>

<span data-ttu-id="b6bd1-132">Nainstalovat službu IIS na *myFrontendVM* pomocí relaci vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-132">You can install IIS on *myFrontendVM* by using a remote desktop session.</span></span> <span data-ttu-id="b6bd1-133">Je třeba tooget hello veřejnou IP adresu hello tooaccess virtuálního počítače jej.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-133">You need tooget hello public IP address of hello VM tooaccess it.</span></span>

<span data-ttu-id="b6bd1-134">Můžete získat hello veřejnou IP adresu *myFrontendVM* s [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="b6bd1-134">You can get hello public IP address of *myFrontendVM* with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="b6bd1-135">Hello následující příklad získá hello IP adresu pro *myPublicIPAddress* vytvořili dříve:</span><span class="sxs-lookup"><span data-stu-id="b6bd1-135">hello following example obtains hello IP address for *myPublicIPAddress* created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName myRGNetwork `
    -Name myPublicIPAddress | select IpAddress
```

<span data-ttu-id="b6bd1-136">Poznamenejte si tuto IP adresu, můžete ji použít v budoucnu krocích.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-136">Take note of this IP Address so you can use it in future steps.</span></span>

<span data-ttu-id="b6bd1-137">Použití hello následující příkaz toocreate a relace vzdálené plochy s *myFrontendVM*.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-137">Use hello following command toocreate a remote desktop session with *myFrontendVM*.</span></span> <span data-ttu-id="b6bd1-138">Nahraďte  *<publicIPAddress>*  hello adresy, která jste si předtím poznamenali.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-138">Replace *<publicIPAddress>* with hello address that you previously recorded.</span></span> <span data-ttu-id="b6bd1-139">Po zobrazení výzvy zadejte přihlašovací údaje hello při vytvoření hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-139">When prompted, enter hello credentials used when you created hello VM.</span></span>

```
mstsc /v:<publicIpAddress>
``` 

<span data-ttu-id="b6bd1-140">Teď, když jste přihlášení příliš*myFrontendVM*, můžete použít jeden řádek tooinstall prostředí PowerShell služby IIS a povolit hello místní brány firewall pravidla tooallow webový provoz.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-140">Now that you have logged in too*myFrontendVM*, you can use a single line of PowerShell tooinstall IIS and enable hello local firewall rule tooallow web traffic.</span></span> <span data-ttu-id="b6bd1-141">Otevřete příkazovém řádku prostředí PowerShell a spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="b6bd1-141">Open a PowerShell prompt and run hello following command:</span></span>

<span data-ttu-id="b6bd1-142">Použití [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/servermanager/install-windowsfeature) rozšíření vlastních skriptů hello toorun, který instaluje webový server IIS hello:</span><span class="sxs-lookup"><span data-stu-id="b6bd1-142">Use [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/servermanager/install-windowsfeature) toorun hello custom script extension that installs hello IIS webserver:</span></span>

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

<span data-ttu-id="b6bd1-143">Teď můžete použít hello veřejnou IP adresu toobrowse webu IIS toohello virtuálních počítačů toosee hello.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-143">Now you can use hello public IP address toobrowse toohello VM toosee hello IIS site.</span></span>

![Výchozí web služby IIS](./media/tutorial-virtual-network/iis.png)

## <a name="manage-internal-traffic"></a><span data-ttu-id="b6bd1-145">Spravovat interní provoz</span><span class="sxs-lookup"><span data-stu-id="b6bd1-145">Manage internal traffic</span></span>

<span data-ttu-id="b6bd1-146">Skupina zabezpečení sítě (NSG) obsahuje seznam pravidel zabezpečení, která povolují nebo odepírají síťový provoz tooresources připojené tooa virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-146">A network security group (NSG) contains a list of security rules that allow or deny network traffic tooresources connected tooa VNet.</span></span> <span data-ttu-id="b6bd1-147">Skupiny Nsg můžou být přidružené toosubnets nebo jednotlivé síťové adaptéry připojené tooVMs.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-147">NSGs can be associated toosubnets or individual NICs attached tooVMs.</span></span> <span data-ttu-id="b6bd1-148">Otevírání nebo při zavření tooVMs přístup prostřednictvím portů se provádí pomocí pravidla NSG.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-148">Opening or closing access tooVMs through ports is done using NSG rules.</span></span> <span data-ttu-id="b6bd1-149">Při vytváření *myFrontendVM*, příchozí port 3389 se automaticky otevře pro připojení RDP.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-149">When you created *myFrontendVM*, inbound port 3389 was automatically opened for RDP connectivity.</span></span>

<span data-ttu-id="b6bd1-150">Interní komunikaci virtuálních počítačů můžete konfigurovat pomocí skupinu NSG.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-150">Internal communication of VMs can be configured using an NSG.</span></span> <span data-ttu-id="b6bd1-151">V této části se dozvíte, jak toocreate podsíť další v hello sítě a přiřadit tooallow tooit NSG připojení z *myFrontendVM* příliš*myBackendVM* na portu 1433.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-151">In this section, you learn how toocreate an additional subnet in hello network and assign an NSG tooit tooallow a connection from *myFrontendVM* too*myBackendVM* on port 1433.</span></span> <span data-ttu-id="b6bd1-152">podsíť Hello byl přiřazen toohello virtuálního počítače při jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-152">hello subnet is then assigned toohello VM when it is created.</span></span>

<span data-ttu-id="b6bd1-153">Můžete omezit přenosy interní příliš*myBackendVM* z jenom *myFrontendVM* tak, že vytvoříte skupinu NSG pro podsíť hello back-end.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-153">You can limit internal traffic too*myBackendVM* from only *myFrontendVM* by creating an NSG for hello back-end subnet.</span></span> <span data-ttu-id="b6bd1-154">Hello následující příklad vytvoří pravidlo NSG s názvem *myBackendNSGRule* s [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig):</span><span class="sxs-lookup"><span data-stu-id="b6bd1-154">hello following example creates an NSG rule named *myBackendNSGRule* with [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig):</span></span>

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

<span data-ttu-id="b6bd1-155">Přidat skupinu zabezpečení sítě s názvem *myBackendNSG* s [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):</span><span class="sxs-lookup"><span data-stu-id="b6bd1-155">Add a network security group named *myBackendNSG* with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):</span></span>

```powershell
$nsgBackend = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNSG `
  -SecurityRules $nsgBackendRule
```
## <a name="add-back-end-subnet"></a><span data-ttu-id="b6bd1-156">Přidat podsíť back-end</span><span class="sxs-lookup"><span data-stu-id="b6bd1-156">Add back-end subnet</span></span>

<span data-ttu-id="b6bd1-157">Přidat *myBackEndSubnet* příliš*myVNet* s [přidat AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="b6bd1-157">Add *myBackEndSubnet* too*myVNet* with [Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig):</span></span>

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

## <a name="create-back-end-vm"></a><span data-ttu-id="b6bd1-158">Vytvoření virtuálního počítače s back-end</span><span class="sxs-lookup"><span data-stu-id="b6bd1-158">Create back-end VM</span></span>

<span data-ttu-id="b6bd1-159">Hello nejjednodušší způsob, jak toocreate hello back-end virtuální počítač pomocí bitové kopie systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-159">hello easiest way toocreate hello back-end VM is by using a SQL Server image.</span></span> <span data-ttu-id="b6bd1-160">V tomto kurzu jenom vytvoří hello virtuálních počítačů s hello databázový server, ale neposkytuje informace o přístup k databázi hello.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-160">This tutorial only creates hello VM with hello database server, but doesn't provide information about accessing hello database.</span></span>

<span data-ttu-id="b6bd1-161">Vytvoření *myBackendNic*:</span><span class="sxs-lookup"><span data-stu-id="b6bd1-161">Create *myBackendNic*:</span></span>

```powershell
$backendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNic `
  -SubnetId $vnet.Subnets[1].Id
```

<span data-ttu-id="b6bd1-162">Nastavte hello uživatelské jméno a heslo pro účet správce hello na hello virtuálního počítače s Get-Credential potřebné:</span><span class="sxs-lookup"><span data-stu-id="b6bd1-162">Set hello username and password needed for hello administrator account on hello VM with Get-Credential:</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="b6bd1-163">Vytvoření *myBackendVM*:</span><span class="sxs-lookup"><span data-stu-id="b6bd1-163">Create *myBackendVM*:</span></span>

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

<span data-ttu-id="b6bd1-164">Hello bitovou kopii, která se používá nainstalován server SQL, ale není použit v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-164">hello image that is used has SQL Server installed, but is not used in this tutorial.</span></span> <span data-ttu-id="b6bd1-165">Je součástí tooshow můžete konfiguraci aplikace virtuálních počítačů toohandle webového provozu a správy virtuálních počítačů toohandle databáze.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-165">It is included tooshow you how you can configure a VM toohandle web traffic and a VM toohandle database management.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b6bd1-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b6bd1-166">Next steps</span></span>

<span data-ttu-id="b6bd1-167">V tomto kurzu jste vytvořili a zabezpečené sítě Azure jako související toovirtual počítače.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-167">In this tutorial, you created and secured Azure networks as related toovirtual machines.</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="b6bd1-168">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="b6bd1-168">Create a virtual network</span></span>
> * <span data-ttu-id="b6bd1-169">Vytvoření podsítě virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="b6bd1-169">Create virtual network subnets</span></span>
> * <span data-ttu-id="b6bd1-170">Ovládací prvek síťový provoz se skupinami zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="b6bd1-170">Control network traffic with Network Security Groups</span></span>
> * <span data-ttu-id="b6bd1-171">Zobrazení pravidla pro provoz v akci</span><span class="sxs-lookup"><span data-stu-id="b6bd1-171">View traffic rules in action</span></span>

<span data-ttu-id="b6bd1-172">Posunutí další kurz toolearn toohello o monitorování zabezpečení dat na virtuální počítače pomocí zálohování Azure.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-172">Advance toohello next tutorial toolearn about monitoring securing data on virtual machines using Azure backup.</span></span> <span data-ttu-id="b6bd1-173">.</span><span class="sxs-lookup"><span data-stu-id="b6bd1-173">.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b6bd1-174">Zálohovat virtuální počítače s Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="b6bd1-174">Back up Windows virtual machines in Azure</span></span>](./tutorial-backup-vms.md)
