---
title: "Simulované hybridní cloud testovací prostředí | Microsoft Docs"
description: "Vytvoření simulovaného hybridní cloudové prostředí pro IT pro nebo vývoj testování, pomocí dvou virtuálních sítí Azure a připojení VNet-to-VNet."
services: virtual-machines-windows
documentationcenter: 
author: JoeDavies-MSFT
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: ca5bf294-8172-44a9-8fed-d6f38d345364
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: josephd
ms.openlocfilehash: 4ec6f079b762a25894d822bfc098ea5442a1f7e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-a-simulated-hybrid-cloud-environment-for-testing"></a><span data-ttu-id="cf375-103">Nastavení simulovaného hybridního cloudového prostředí pro testování</span><span class="sxs-lookup"><span data-stu-id="cf375-103">Set up a simulated hybrid cloud environment for testing</span></span>
<span data-ttu-id="cf375-104">Tento článek vás provede jednotlivými kroky vytváření simulované hybridní prostředí cloudu s Microsoft Azure pomocí dvou virtuálních sítí Azure.</span><span class="sxs-lookup"><span data-stu-id="cf375-104">This article steps you through creating a simulated hybrid cloud environment with Microsoft Azure using two Azure virtual networks.</span></span> <span data-ttu-id="cf375-105">Zde je Výsledná konfigurace.</span><span class="sxs-lookup"><span data-stu-id="cf375-105">Here is the resulting configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

<span data-ttu-id="cf375-106">To simuluje provozním prostředí hybridní cloud a skládá se z:</span><span class="sxs-lookup"><span data-stu-id="cf375-106">This simulates a hybrid cloud production environment and consists of:</span></span>

* <span data-ttu-id="cf375-107">Simulované a zjednodušenou místní síť hostované v Azure virtuální sítě (virtuální sítě testovací laboratoř).</span><span class="sxs-lookup"><span data-stu-id="cf375-107">A simulated and simplified on-premises network hosted in an Azure virtual network (the TestLab virtual network).</span></span>
* <span data-ttu-id="cf375-108">Simulované mezi různými místy virtuální síť hostované v Azure (TestVNET).</span><span class="sxs-lookup"><span data-stu-id="cf375-108">A simulated cross-premises virtual network hosted in Azure (TestVNET).</span></span>
* <span data-ttu-id="cf375-109">Připojení VNet-to-VNet mezi dvěma virtuálními sítěmi.</span><span class="sxs-lookup"><span data-stu-id="cf375-109">A VNet-to-VNet connection between the two virtual networks.</span></span>
* <span data-ttu-id="cf375-110">Sekundární řadič domény ve virtuální síti TestVNET.</span><span class="sxs-lookup"><span data-stu-id="cf375-110">A secondary domain controller in the TestVNET virtual network.</span></span>

<span data-ttu-id="cf375-111">To poskytuje že základ a běžné od bodu, ze kterých lze:</span><span class="sxs-lookup"><span data-stu-id="cf375-111">This provides a basis and common starting point from which you can:</span></span>

* <span data-ttu-id="cf375-112">Vývoj a testování aplikací v simulovaném hybridní cloudové prostředí.</span><span class="sxs-lookup"><span data-stu-id="cf375-112">Develop and test applications in a simulated hybrid cloud environment.</span></span>
* <span data-ttu-id="cf375-113">Vytváření konfigurací testů počítačů, některé v rámci virtuální sítě testovací laboratoř a některé v rámci TestVNET virtuální sítě, aby simuloval hybridní cloudové IT úlohy.</span><span class="sxs-lookup"><span data-stu-id="cf375-113">Create test configurations of computers, some within the TestLab virtual network and some within the TestVNET virtual network, to simulate hybrid cloud-based IT workloads.</span></span>

<span data-ttu-id="cf375-114">Existují čtyři hlavní fáze k nastavení tohoto testovacího prostředí hybridního cloudu:</span><span class="sxs-lookup"><span data-stu-id="cf375-114">There are four major phases to setting up this hybrid cloud test environment:</span></span>

1. <span data-ttu-id="cf375-115">Nakonfigurujte virtuální síť testovací laboratoř.</span><span class="sxs-lookup"><span data-stu-id="cf375-115">Configure the TestLab virtual network.</span></span>
2. <span data-ttu-id="cf375-116">Vytvoření virtuální sítě mezi různými místy.</span><span class="sxs-lookup"><span data-stu-id="cf375-116">Create the cross-premises virtual network.</span></span>
3. <span data-ttu-id="cf375-117">Vytvoření připojení k síti VPN VNet-to-VNet.</span><span class="sxs-lookup"><span data-stu-id="cf375-117">Create the VNet-to-VNet VPN connection.</span></span>
4. <span data-ttu-id="cf375-118">Konfigurace počítače DC2.</span><span class="sxs-lookup"><span data-stu-id="cf375-118">Configure DC2.</span></span> 

<span data-ttu-id="cf375-119">Tato konfigurace vyžaduje předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="cf375-119">This configuration requires an Azure subscription.</span></span> <span data-ttu-id="cf375-120">Pokud máte předplatné MSDN nebo v sadě Visual Studio, najdete v části [platební měsíční Azure pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="cf375-120">If you have an MSDN or Visual Studio subscription, see [Monthly Azure credit for Visual Studio subscribers](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

> [!NOTE]
> <span data-ttu-id="cf375-121">Virtuální počítače a brány virtuální sítě v Azure zpoplatněná nákladů na probíhající peněžní při spuštění.</span><span class="sxs-lookup"><span data-stu-id="cf375-121">Virtual machines and virtual network gateways in Azure incur an ongoing monetary cost when they are running.</span></span> <span data-ttu-id="cf375-122">Tyto poplatky je účtován na vašem webu MSDN nebo placené předplatné.</span><span class="sxs-lookup"><span data-stu-id="cf375-122">This cost is billed against your MSDN or paid subscription.</span></span> <span data-ttu-id="cf375-123">Služby Azure VPN gateway je implementovaná jako sada dva virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="cf375-123">An Azure VPN gateway is implemented as a set of two Azure virtual machines.</span></span> <span data-ttu-id="cf375-124">Chcete-li minimalizovat náklady, vytvořit testovací prostředí a provádět potřebné testováním a ukázkový co nejrychleji.</span><span class="sxs-lookup"><span data-stu-id="cf375-124">To minimize the costs, create the test environment and perform your needed testing and demonstration as quickly as possible.</span></span>
> 
> 

## <a name="phase-1-configure-the-testlab-virtual-network"></a><span data-ttu-id="cf375-125">Fáze 1: Konfigurace virtuální sítě testovací laboratoř</span><span class="sxs-lookup"><span data-stu-id="cf375-125">Phase 1: Configure the TestLab virtual network</span></span>
<span data-ttu-id="cf375-126">Postupujte podle pokynů v [základní konfigurace testovacího prostředí](https://technet.microsoft.com/library/mt771177.aspx) tématu ke konfiguraci počítačů DC1, APP1 a CLIENT1 ve virtuální síti Azure, s názvem testovací laboratoř.</span><span class="sxs-lookup"><span data-stu-id="cf375-126">Use the instructions in the [Base Configuration test environment](https://technet.microsoft.com/library/mt771177.aspx) topic to configure the DC1, APP1, and CLIENT1 computers in the Azure virtual network named TestLab.</span></span> 

<span data-ttu-id="cf375-127">V dalším kroku spusťte příkazovém řádku prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cf375-127">Next, start an Azure PowerShell prompt.</span></span>

> [!NOTE]
> <span data-ttu-id="cf375-128">Tento příkaz nastaví použití Azure PowerShell 1.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="cf375-128">The following command sets use Azure PowerShell 1.0 and later.</span></span>
> 
> 

<span data-ttu-id="cf375-129">Přihlaste se ke svému účtu.</span><span class="sxs-lookup"><span data-stu-id="cf375-129">Sign in to your account.</span></span>

    Login-AzureRMAccount

<span data-ttu-id="cf375-130">Získáte název odběru, pomocí následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="cf375-130">Get your subscription name using the following command.</span></span>

    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName

<span data-ttu-id="cf375-131">Nastavte vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="cf375-131">Set your Azure subscription.</span></span> <span data-ttu-id="cf375-132">Použijte stejné předplatné, které jste použili k vytvoření základní konfigurace fáze 1.</span><span class="sxs-lookup"><span data-stu-id="cf375-132">Use the same subscription that you used to build your base configuration in Phase 1.</span></span> <span data-ttu-id="cf375-133">Nahraďte všechna data v uvozovkách, včetně < a > znaků, se správným názvem.</span><span class="sxs-lookup"><span data-stu-id="cf375-133">Replace everything within the quotes, including the < and > characters, with the correct name.</span></span>

    $subscr="<subscription name>"
    Get-AzureRmSubscription –SubscriptionName $subscr | Select-AzureRmSubscription

<span data-ttu-id="cf375-134">Dále přidáte podsíť brány virtuální sítě testovací laboratoř základní konfigurace, který bude použit k hostování bránu Azure.</span><span class="sxs-lookup"><span data-stu-id="cf375-134">Next, add a gateway subnet to the TestLab virtual network of your base configuration, which will be used to host the Azure gateway.</span></span>

    $rgName="<name of your resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $vnet=Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name TestLab
    Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 10.255.255.248/29 -VirtualNetwork $vnet
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

<span data-ttu-id="cf375-135">V dalším kroku žádost veřejnou IP adresu přiřadit k bráně virtuální sítě testovací laboratoř.</span><span class="sxs-lookup"><span data-stu-id="cf375-135">Next, request a public IP address to assign to the gateway for the TestLab virtual network.</span></span>

    $gwpip=New-AzureRmPublicIpAddress -Name TestLab_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic

<span data-ttu-id="cf375-136">Dál vytvořte bránu.</span><span class="sxs-lookup"><span data-stu-id="cf375-136">Next, create your gateway.</span></span>

    $vnet=Get-AzureRmVirtualNetwork -Name TestLab -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name TestLab_GWConfig -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 
    New-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

<span data-ttu-id="cf375-137">Mějte na paměti, že nové brány může trvat 20 minut nebo déle vytvořit.</span><span class="sxs-lookup"><span data-stu-id="cf375-137">Keep in mind that new gateways can take 20 minutes or more to create.</span></span>

<span data-ttu-id="cf375-138">Z portálu Azure k místnímu počítači připojte k řadiči domény DC1 se přihlašovací údaje pro corp\uživatel1.</span><span class="sxs-lookup"><span data-stu-id="cf375-138">From the Azure portal on your local computer, connect to DC1 with the CORP\User1 credentials.</span></span> <span data-ttu-id="cf375-139">Konfigurovat domény CORP tak, aby počítače a uživatelé používat své místní řadič domény pro ověřování, spusťte tyto příkazy z příkazového řádku prostředí Windows PowerShell na úrovni správce na počítači DC1.</span><span class="sxs-lookup"><span data-stu-id="cf375-139">To configure the CORP domain so that computers and users use their local domain controller for authentication, run these commands from an administrator-level Windows PowerShell command prompt on DC1.</span></span>

    New-ADReplicationSite -Name "TestLab" 
    New-ADReplicationSite -Name "TestVNET"
    New-ADReplicationSubnet -Name "10.0.0.0/8" -Site "TestLab"
    New-ADReplicationSubnet -Name "192.168.0.0/16" -Site "TestVNET"

<span data-ttu-id="cf375-140">Toto je aktuální konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="cf375-140">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph1.png)

## <a name="phase-2-create-the-testvnet-virtual-network"></a><span data-ttu-id="cf375-141">Fáze 2: Vytvoření virtuální sítě TestVNET</span><span class="sxs-lookup"><span data-stu-id="cf375-141">Phase 2: Create the TestVNET virtual network</span></span>
<span data-ttu-id="cf375-142">Nejdřív vytvoříte virtuální síť TestVNET a chránit pomocí skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="cf375-142">First, create the TestVNET virtual network and protect it with a network security group.</span></span>

    $rgName="<name of the resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $locShortName="<Azure location name from $locName in all lowercase letters with spaces removed. Example:  westus>"
    $testSubnet=New-AzureRMVirtualNetworkSubnetConfig -Name "TestSubnet" -AddressPrefix 192.168.0.0/24
    $gatewaySubnet=New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 192.168.255.248/29
    New-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName -Location $locName -AddressPrefix 192.168.0.0/16 -Subnet $testSubnet,$gatewaySubnet –DNSServer 10.0.0.4
    $rule1=New-AzureRMNetworkSecurityRuleConfig -Name "RDPTraffic" -Description "Allow RDP to all VMs on the subnet" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389
    New-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName -Location $locShortName -SecurityRules $rule1
    $vnet=Get-AzureRMVirtualNetwork -ResourceGroupName $rgName -Name TestVNET
    $nsg=Get-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName
    Set-AzureRMVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet" -AddressPrefix 192.168.0.0/24 -NetworkSecurityGroup $nsg

<span data-ttu-id="cf375-143">V dalším kroku žádost veřejnou adresu IP, která bude přidělena pro bránu virtuální sítě TestVNET a vytvořte bránu.</span><span class="sxs-lookup"><span data-stu-id="cf375-143">Next, request a public IP address to be allocated to the gateway for the TestVNET virtual network and create your gateway.</span></span>

    $gwpip=New-AzureRmPublicIpAddress -Name TestVNET_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $vnet=Get-AzureRmVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name "TestVNET_GWConfig" -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
    New-AzureRmVirtualNetworkGateway -Name "TestVNET_GW" -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

<span data-ttu-id="cf375-144">Toto je aktuální konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="cf375-144">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph2.png)

## <a name="phase-3-create-the-vnet-to-vnet-connection"></a><span data-ttu-id="cf375-145">Fáze 3: Vytvoření připojení VNet-to-VNet</span><span class="sxs-lookup"><span data-stu-id="cf375-145">Phase 3: Create the VNet-to-VNet connection</span></span>
<span data-ttu-id="cf375-146">Nejdřív získejte náhodné, kryptograficky silnou, 32 znaků předsdílený klíč ze správce sítě nebo zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="cf375-146">First, obtain a random, cryptographically strong, 32-character pre-shared key from your network or security administrator.</span></span> <span data-ttu-id="cf375-147">Alternativně použijte informace v [vytvořit náhodný řetězec pro předsdílený klíč pomocí protokolu IPsec](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) získat předsdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="cf375-147">Alternately, use the information at [Create a random string for an IPsec preshared key](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) to obtain a pre-shared key.</span></span>

<span data-ttu-id="cf375-148">Dále použijte tyto příkazy k vytvoření připojení VPN VNet-to-VNet, což může trvat delší dobu.</span><span class="sxs-lookup"><span data-stu-id="cf375-148">Next, use these commands to create the VNet-to-VNet VPN connection, which can take some time to complete.</span></span>

    $sharedKey="<pre-shared key value>"
    $gwTestLab=Get-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName
    $gwTestVNET=Get-AzureRmVirtualNetworkGateway -Name TestVNET_GW -ResourceGroupName $rgName
    New-AzureRmVirtualNetworkGatewayConnection -Name TestLab_to_TestVNET -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestLab -VirtualNetworkGateway2 $gwTestVNET -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey
    New-AzureRmVirtualNetworkGatewayConnection -Name TestVNET_to_TestLab -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestVNET -VirtualNetworkGateway2 $gwTestLab -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey

<span data-ttu-id="cf375-149">Po několika minutách by se mělo vytvořit připojení.</span><span class="sxs-lookup"><span data-stu-id="cf375-149">After a few minutes, the connection should be established.</span></span>

<span data-ttu-id="cf375-150">Toto je aktuální konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="cf375-150">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph3.png)

## <a name="phase-4-configure-dc2"></a><span data-ttu-id="cf375-151">Fáze 4: Konfigurace počítače DC2</span><span class="sxs-lookup"><span data-stu-id="cf375-151">Phase 4: Configure DC2</span></span>
<span data-ttu-id="cf375-152">Nejprve vytvořte virtuálního počítače pro řadič domény DC2.</span><span class="sxs-lookup"><span data-stu-id="cf375-152">First, create a virtual machine for DC2.</span></span> <span data-ttu-id="cf375-153">Spuštění těchto příkazů na příkazovém řádku prostředí Azure PowerShell v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="cf375-153">Run these commands at the Azure PowerShell command prompt on your local computer.</span></span>

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<the storage account name for the base configuration>"
    $vnet=Get-AzureRMVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $pip=New-AzureRMPublicIpAddress -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -PrivateIpAddress 192.168.0.4
    $vm=New-AzureRMVMConfig -VMName DC2 -VMSize Standard_A1
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestVNET-ADDSDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name ADDS-Data -DiskSizeInGB 20 -VhdUri $vhdURI  -CreateOption empty
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for DC2."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName DC2 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name DC2-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

<span data-ttu-id="cf375-154">V dalším kroku připojte k nového virtuálního počítače DC2 z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="cf375-154">Next, connect to the new DC2 virtual machine from the Azure portal.</span></span>

<span data-ttu-id="cf375-155">V dalším kroku nakonfigurujte brány Windows Firewall pravidlo umožňující přenos pro testování základní připojení.</span><span class="sxs-lookup"><span data-stu-id="cf375-155">Next, configure a Windows Firewall rule to allow traffic for basic connectivity testing.</span></span> <span data-ttu-id="cf375-156">Z prostředí Windows PowerShell na úrovni správce příkazového řádku na počítači DC2 spuštění těchto příkazů.</span><span class="sxs-lookup"><span data-stu-id="cf375-156">From an administrator-level Windows PowerShell command prompt on DC2, run these commands.</span></span>

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc1.corp.contoso.com

<span data-ttu-id="cf375-157">Příkaz ping by měl mít za následek čtyři úspěšné odpovědi z IP adresy 10.0.0.4.</span><span class="sxs-lookup"><span data-stu-id="cf375-157">The ping command should result in four successful replies from IP address 10.0.0.4.</span></span> <span data-ttu-id="cf375-158">Toto je test provozu prostřednictvím připojení VNet-to-VNet.</span><span class="sxs-lookup"><span data-stu-id="cf375-158">This is a test of traffic across the VNet-to-VNet connection.</span></span>

<span data-ttu-id="cf375-159">V dalším kroku přidejte disk doplňující data na počítači DC2 jako nový svazek s písmenem jednotky F:.</span><span class="sxs-lookup"><span data-stu-id="cf375-159">Next, add the extra data disk on DC2 as a new volume with the drive letter F:.</span></span>

1. <span data-ttu-id="cf375-160">V levém podokně ve Správci serveru klikněte na tlačítko **Souborová služba a služba úložiště**a potom klikněte na **disky**.</span><span class="sxs-lookup"><span data-stu-id="cf375-160">In the left pane of Server Manager, click **File and Storage Services**, and then click **Disks**.</span></span>
2. <span data-ttu-id="cf375-161">V podokně obsahu v **disky** klikněte na možnost **disku 2** (s **oddílu** nastavena na **neznámé**).</span><span class="sxs-lookup"><span data-stu-id="cf375-161">In the contents pane, in the **Disks** group, click **disk 2** (with the **Partition** set to **Unknown**).</span></span>
3. <span data-ttu-id="cf375-162">Klikněte na tlačítko **úlohy**a potom klikněte na **nový svazek**.</span><span class="sxs-lookup"><span data-stu-id="cf375-162">Click **Tasks**, and then click **New Volume**.</span></span>
4. <span data-ttu-id="cf375-163">Na stránce než můžete začít stránky Průvodce vytvořením svazku, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="cf375-163">On the Before you begin page of the New Volume Wizard, click **Next**.</span></span>
5. <span data-ttu-id="cf375-164">Na stránce vyberte server a disk stránky, klikněte na tlačítko **disku 2**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="cf375-164">On the Select the server and disk page, click **Disk 2**, and then click **Next**.</span></span> <span data-ttu-id="cf375-165">Po zobrazení výzvy klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="cf375-165">When prompted, click **OK**.</span></span>
6. <span data-ttu-id="cf375-166">Na zadání velikosti svazku stránky, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="cf375-166">On the Specify the size of the volume page, click **Next**.</span></span>
7. <span data-ttu-id="cf375-167">Na přiřazení disku písmeno nebo složka stránku, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="cf375-167">On the Assign to a drive letter or folder page, click **Next**.</span></span>
8. <span data-ttu-id="cf375-168">Na stránce nastavení systému vybrat soubor, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="cf375-168">On the Select file system settings page, click **Next**.</span></span>
9. <span data-ttu-id="cf375-169">Na stránce Potvrdit vybrané možnosti, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cf375-169">On the Confirm selections page, click **Create**.</span></span>
10. <span data-ttu-id="cf375-170">Po dokončení klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="cf375-170">When complete, click **Close**.</span></span>

<span data-ttu-id="cf375-171">V dalším kroku nakonfigurujte DC2 jako repliku řadiče domény v doméně corp.contoso.com.</span><span class="sxs-lookup"><span data-stu-id="cf375-171">Next, configure DC2 as a replica domain controller for the corp.contoso.com domain.</span></span> <span data-ttu-id="cf375-172">Spusťte tyto příkazy z příkazového řádku prostředí Windows PowerShell na počítači DC2.</span><span class="sxs-lookup"><span data-stu-id="cf375-172">Run these commands from the Windows PowerShell command prompt on DC2.</span></span>

    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -Credential (Get-Credential CORP\User1) -DomainName "corp.contoso.com" -InstallDns:$true -DatabasePath "F:\NTDS" -LogPath "F:\Logs" -SysvolPath "F:\SYSVOL"

<span data-ttu-id="cf375-173">Všimněte si, že budete vyzváni k zadání hesla corp\uživatel1 a heslo Directory režimu obnovení služeb (DSRM) a k restartování počítače DC2.</span><span class="sxs-lookup"><span data-stu-id="cf375-173">Note that you are prompted to supply both the CORP\User1 password and a Directory Services Restore Mode (DSRM) password, and to restart DC2.</span></span>

<span data-ttu-id="cf375-174">Nyní, když virtuální sítě TestVNET vlastní server DNS (DC2), je nutné nakonfigurovat virtuální síť TestVNET používat tento server DNS.</span><span class="sxs-lookup"><span data-stu-id="cf375-174">Now that the TestVNET virtual network has its own DNS server (DC2), you must configure the TestVNET virtual network to use this DNS server.</span></span>

1. <span data-ttu-id="cf375-175">V levém podokně portálu Azure, klikněte na ikonu virtuální sítě a pak klikněte na tlačítko **TestVNET**.</span><span class="sxs-lookup"><span data-stu-id="cf375-175">In the left pane of the Azure portal, click the virtual networks icon, and then click **TestVNET**.</span></span>
2. <span data-ttu-id="cf375-176">Na **nastavení** , klikněte na **servery DNS**.</span><span class="sxs-lookup"><span data-stu-id="cf375-176">On the **Settings** tab, click **DNS servers**.</span></span>
3. <span data-ttu-id="cf375-177">V **primárního serveru DNS**, typ **192.168.0.4** nahradit 10.0.0.4.</span><span class="sxs-lookup"><span data-stu-id="cf375-177">In **Primary DNS server**, type **192.168.0.4** to replace 10.0.0.4.</span></span>
4. <span data-ttu-id="cf375-178">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="cf375-178">Click **Save**.</span></span>

<span data-ttu-id="cf375-179">Toto je aktuální konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="cf375-179">This is your current configuration.</span></span> 

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

<span data-ttu-id="cf375-180">Simulované hybridní cloudové prostředí je nyní připraven pro testování.</span><span class="sxs-lookup"><span data-stu-id="cf375-180">Your simulated hybrid cloud environment is now ready for testing.</span></span>

## <a name="next-step"></a><span data-ttu-id="cf375-181">Další krok</span><span class="sxs-lookup"><span data-stu-id="cf375-181">Next step</span></span>
* <span data-ttu-id="cf375-182">Nastavení [webové, řádek obchodní aplikace](ps-hybrid-cloud-test-env-lob.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) v tomto prostředí.</span><span class="sxs-lookup"><span data-stu-id="cf375-182">Set up a [web-based, line of business application](ps-hybrid-cloud-test-env-lob.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) in this environment.</span></span>

