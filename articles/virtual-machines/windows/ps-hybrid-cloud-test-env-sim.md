---
title: "aaaSimulated hybridní cloud testovací prostředí | Microsoft Docs"
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
ms.openlocfilehash: 6063022e373c2b3564e040c4fbee122e2438974b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-simulated-hybrid-cloud-environment-for-testing"></a><span data-ttu-id="56cbe-103">Nastavení simulovaného hybridního cloudového prostředí pro testování</span><span class="sxs-lookup"><span data-stu-id="56cbe-103">Set up a simulated hybrid cloud environment for testing</span></span>
<span data-ttu-id="56cbe-104">Tento článek vás provede jednotlivými kroky vytváření simulované hybridní prostředí cloudu s Microsoft Azure pomocí dvou virtuálních sítí Azure.</span><span class="sxs-lookup"><span data-stu-id="56cbe-104">This article steps you through creating a simulated hybrid cloud environment with Microsoft Azure using two Azure virtual networks.</span></span> <span data-ttu-id="56cbe-105">Zde je hello Výsledná konfigurace.</span><span class="sxs-lookup"><span data-stu-id="56cbe-105">Here is hello resulting configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

<span data-ttu-id="56cbe-106">To simuluje provozním prostředí hybridní cloud a skládá se z:</span><span class="sxs-lookup"><span data-stu-id="56cbe-106">This simulates a hybrid cloud production environment and consists of:</span></span>

* <span data-ttu-id="56cbe-107">Simulované a zjednodušenou místní síť hostované ve virtuální sítě Azure (hello testovací laboratoř virtuální sítě).</span><span class="sxs-lookup"><span data-stu-id="56cbe-107">A simulated and simplified on-premises network hosted in an Azure virtual network (hello TestLab virtual network).</span></span>
* <span data-ttu-id="56cbe-108">Simulované mezi různými místy virtuální síť hostované v Azure (TestVNET).</span><span class="sxs-lookup"><span data-stu-id="56cbe-108">A simulated cross-premises virtual network hosted in Azure (TestVNET).</span></span>
* <span data-ttu-id="56cbe-109">Připojení VNet-to-VNet mezi hello dvě virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="56cbe-109">A VNet-to-VNet connection between hello two virtual networks.</span></span>
* <span data-ttu-id="56cbe-110">Sekundární řadič domény ve virtuální síti TestVNET hello.</span><span class="sxs-lookup"><span data-stu-id="56cbe-110">A secondary domain controller in hello TestVNET virtual network.</span></span>

<span data-ttu-id="56cbe-111">To poskytuje že základ a běžné od bodu, ze kterých lze:</span><span class="sxs-lookup"><span data-stu-id="56cbe-111">This provides a basis and common starting point from which you can:</span></span>

* <span data-ttu-id="56cbe-112">Vývoj a testování aplikací v simulovaném hybridní cloudové prostředí.</span><span class="sxs-lookup"><span data-stu-id="56cbe-112">Develop and test applications in a simulated hybrid cloud environment.</span></span>
* <span data-ttu-id="56cbe-113">Vytváření konfigurací testů počítačů, některé v rámci hello testovací laboratoř virtuální sítě a některé v rámci hello TestVNET virtuální sítě, toosimulate hybridní cloudové IT úlohy.</span><span class="sxs-lookup"><span data-stu-id="56cbe-113">Create test configurations of computers, some within hello TestLab virtual network and some within hello TestVNET virtual network, toosimulate hybrid cloud-based IT workloads.</span></span>

<span data-ttu-id="56cbe-114">Existují čtyři hlavní fáze toosetting toto hybridní cloud testovací prostředí:</span><span class="sxs-lookup"><span data-stu-id="56cbe-114">There are four major phases toosetting up this hybrid cloud test environment:</span></span>

1. <span data-ttu-id="56cbe-115">Nakonfigurujte hello testovací laboratoř virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="56cbe-115">Configure hello TestLab virtual network.</span></span>
2. <span data-ttu-id="56cbe-116">Vytvoření virtuální sítě mezi různými místy hello.</span><span class="sxs-lookup"><span data-stu-id="56cbe-116">Create hello cross-premises virtual network.</span></span>
3. <span data-ttu-id="56cbe-117">Vytvořte připojení VPN hello VNet-to-VNet.</span><span class="sxs-lookup"><span data-stu-id="56cbe-117">Create hello VNet-to-VNet VPN connection.</span></span>
4. <span data-ttu-id="56cbe-118">Konfigurace počítače DC2.</span><span class="sxs-lookup"><span data-stu-id="56cbe-118">Configure DC2.</span></span> 

<span data-ttu-id="56cbe-119">Tato konfigurace vyžaduje předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="56cbe-119">This configuration requires an Azure subscription.</span></span> <span data-ttu-id="56cbe-120">Pokud máte předplatné MSDN nebo v sadě Visual Studio, najdete v části [platební měsíční Azure pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="56cbe-120">If you have an MSDN or Visual Studio subscription, see [Monthly Azure credit for Visual Studio subscribers](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

> [!NOTE]
> <span data-ttu-id="56cbe-121">Virtuální počítače a brány virtuální sítě v Azure zpoplatněná nákladů na probíhající peněžní při spuštění.</span><span class="sxs-lookup"><span data-stu-id="56cbe-121">Virtual machines and virtual network gateways in Azure incur an ongoing monetary cost when they are running.</span></span> <span data-ttu-id="56cbe-122">Tyto poplatky je účtován na vašem webu MSDN nebo placené předplatné.</span><span class="sxs-lookup"><span data-stu-id="56cbe-122">This cost is billed against your MSDN or paid subscription.</span></span> <span data-ttu-id="56cbe-123">Služby Azure VPN gateway je implementovaná jako sada dva virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="56cbe-123">An Azure VPN gateway is implemented as a set of two Azure virtual machines.</span></span> <span data-ttu-id="56cbe-124">toominimize hello náklady, vytvořit hello testovacího prostředí a proveďte potřebné testováním a ukázkový co nejrychleji.</span><span class="sxs-lookup"><span data-stu-id="56cbe-124">toominimize hello costs, create hello test environment and perform your needed testing and demonstration as quickly as possible.</span></span>
> 
> 

## <a name="phase-1-configure-hello-testlab-virtual-network"></a><span data-ttu-id="56cbe-125">Fáze 1: Konfigurace hello testovací laboratoř virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="56cbe-125">Phase 1: Configure hello TestLab virtual network</span></span>
<span data-ttu-id="56cbe-126">Použijte pokyny hello v hello [základní konfigurace testovacího prostředí](https://technet.microsoft.com/library/mt771177.aspx) tématu tooconfigure hello DC1, APP1 a CLIENT1 počítačů v hello virtuální síť Azure s názvem testovací laboratoř.</span><span class="sxs-lookup"><span data-stu-id="56cbe-126">Use hello instructions in hello [Base Configuration test environment](https://technet.microsoft.com/library/mt771177.aspx) topic tooconfigure hello DC1, APP1, and CLIENT1 computers in hello Azure virtual network named TestLab.</span></span> 

<span data-ttu-id="56cbe-127">V dalším kroku spusťte příkazovém řádku prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="56cbe-127">Next, start an Azure PowerShell prompt.</span></span>

> [!NOTE]
> <span data-ttu-id="56cbe-128">Hello následující sady příkazů pomocí Azure PowerShell 1.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="56cbe-128">hello following command sets use Azure PowerShell 1.0 and later.</span></span>
> 
> 

<span data-ttu-id="56cbe-129">Přihlaste se tooyour účtu.</span><span class="sxs-lookup"><span data-stu-id="56cbe-129">Sign in tooyour account.</span></span>

    Login-AzureRMAccount

<span data-ttu-id="56cbe-130">Získáte název odběru pomocí hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="56cbe-130">Get your subscription name using hello following command.</span></span>

    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName

<span data-ttu-id="56cbe-131">Nastavte vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="56cbe-131">Set your Azure subscription.</span></span> <span data-ttu-id="56cbe-132">Použití hello stejné předplatné, kterou jste použili toobuild základní konfigurace v fáze 1.</span><span class="sxs-lookup"><span data-stu-id="56cbe-132">Use hello same subscription that you used toobuild your base configuration in Phase 1.</span></span> <span data-ttu-id="56cbe-133">Vše v rámci hello uvozovky, včetně hello < a > znaky, nahraďte hello správný název.</span><span class="sxs-lookup"><span data-stu-id="56cbe-133">Replace everything within hello quotes, including hello < and > characters, with hello correct name.</span></span>

    $subscr="<subscription name>"
    Get-AzureRmSubscription –SubscriptionName $subscr | Select-AzureRmSubscription

<span data-ttu-id="56cbe-134">Dál přidejte brány podsítě toohello testovací laboratoř virtuální síť základní konfigurace, které budou použité toohost hello služba Azure gateway.</span><span class="sxs-lookup"><span data-stu-id="56cbe-134">Next, add a gateway subnet toohello TestLab virtual network of your base configuration, which will be used toohost hello Azure gateway.</span></span>

    $rgName="<name of your resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed hello TestLab virtual network, such as West US>"
    $vnet=Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name TestLab
    Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 10.255.255.248/29 -VirtualNetwork $vnet
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

<span data-ttu-id="56cbe-135">V dalším kroku požádat o veřejné IP adresy tooassign toohello bránu pro virtuální síť testovací laboratoř hello.</span><span class="sxs-lookup"><span data-stu-id="56cbe-135">Next, request a public IP address tooassign toohello gateway for hello TestLab virtual network.</span></span>

    $gwpip=New-AzureRmPublicIpAddress -Name TestLab_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic

<span data-ttu-id="56cbe-136">Dál vytvořte bránu.</span><span class="sxs-lookup"><span data-stu-id="56cbe-136">Next, create your gateway.</span></span>

    $vnet=Get-AzureRmVirtualNetwork -Name TestLab -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name TestLab_GWConfig -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 
    New-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

<span data-ttu-id="56cbe-137">Uvědomte si, že nové brány může trvat 20 minut nebo další toocreate.</span><span class="sxs-lookup"><span data-stu-id="56cbe-137">Keep in mind that new gateways can take 20 minutes or more toocreate.</span></span>

<span data-ttu-id="56cbe-138">Z hello k portálu Azure do místního počítače, připojení tooDC1 hello přihlašovací údaje pro corp\uživatel1.</span><span class="sxs-lookup"><span data-stu-id="56cbe-138">From hello Azure portal on your local computer, connect tooDC1 with hello CORP\User1 credentials.</span></span> <span data-ttu-id="56cbe-139">domény CORP hello tooconfigure tak, aby počítače a uživatelé používat své místní řadič domény pro ověřování, spusťte tyto příkazy z příkazového řádku prostředí Windows PowerShell na úrovni správce na počítači DC1.</span><span class="sxs-lookup"><span data-stu-id="56cbe-139">tooconfigure hello CORP domain so that computers and users use their local domain controller for authentication, run these commands from an administrator-level Windows PowerShell command prompt on DC1.</span></span>

    New-ADReplicationSite -Name "TestLab" 
    New-ADReplicationSite -Name "TestVNET"
    New-ADReplicationSubnet -Name "10.0.0.0/8" -Site "TestLab"
    New-ADReplicationSubnet -Name "192.168.0.0/16" -Site "TestVNET"

<span data-ttu-id="56cbe-140">Toto je aktuální konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="56cbe-140">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph1.png)

## <a name="phase-2-create-hello-testvnet-virtual-network"></a><span data-ttu-id="56cbe-141">Fáze 2: Vytvoření virtuální sítě TestVNET hello</span><span class="sxs-lookup"><span data-stu-id="56cbe-141">Phase 2: Create hello TestVNET virtual network</span></span>
<span data-ttu-id="56cbe-142">Nejprve vytvořte hello TestVNET virtuální sítě a chránit pomocí skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="56cbe-142">First, create hello TestVNET virtual network and protect it with a network security group.</span></span>

    $rgName="<name of hello resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed hello TestLab virtual network, such as West US>"
    $locShortName="<Azure location name from $locName in all lowercase letters with spaces removed. Example:  westus>"
    $testSubnet=New-AzureRMVirtualNetworkSubnetConfig -Name "TestSubnet" -AddressPrefix 192.168.0.0/24
    $gatewaySubnet=New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 192.168.255.248/29
    New-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName -Location $locName -AddressPrefix 192.168.0.0/16 -Subnet $testSubnet,$gatewaySubnet –DNSServer 10.0.0.4
    $rule1=New-AzureRMNetworkSecurityRuleConfig -Name "RDPTraffic" -Description "Allow RDP tooall VMs on hello subnet" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389
    New-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName -Location $locShortName -SecurityRules $rule1
    $vnet=Get-AzureRMVirtualNetwork -ResourceGroupName $rgName -Name TestVNET
    $nsg=Get-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName
    Set-AzureRMVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet" -AddressPrefix 192.168.0.0/24 -NetworkSecurityGroup $nsg

<span data-ttu-id="56cbe-143">Žádost o veřejný toobe adres IP v dalším kroku přidělené toohello brány virtuální sítě hello TestVNET a vytvořte bránu.</span><span class="sxs-lookup"><span data-stu-id="56cbe-143">Next, request a public IP address toobe allocated toohello gateway for hello TestVNET virtual network and create your gateway.</span></span>

    $gwpip=New-AzureRmPublicIpAddress -Name TestVNET_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $vnet=Get-AzureRmVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name "TestVNET_GWConfig" -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
    New-AzureRmVirtualNetworkGateway -Name "TestVNET_GW" -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

<span data-ttu-id="56cbe-144">Toto je aktuální konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="56cbe-144">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph2.png)

## <a name="phase-3-create-hello-vnet-to-vnet-connection"></a><span data-ttu-id="56cbe-145">Fáze 3: Vytvoření připojení hello VNet-to-VNet</span><span class="sxs-lookup"><span data-stu-id="56cbe-145">Phase 3: Create hello VNet-to-VNet connection</span></span>
<span data-ttu-id="56cbe-146">Nejdřív získejte náhodné, kryptograficky silnou, 32 znaků předsdílený klíč ze správce sítě nebo zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="56cbe-146">First, obtain a random, cryptographically strong, 32-character pre-shared key from your network or security administrator.</span></span> <span data-ttu-id="56cbe-147">Můžete také použít informace hello [vytvořit náhodný řetězec pro předsdílený klíč pomocí protokolu IPsec](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) tooobtain předsdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="56cbe-147">Alternately, use hello information at [Create a random string for an IPsec preshared key](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) tooobtain a pre-shared key.</span></span>

<span data-ttu-id="56cbe-148">Pak pomocí těchto příkazů toocreate hello VPN VNet-to-VNet připojení, které můžete provést některé toocomplete čas.</span><span class="sxs-lookup"><span data-stu-id="56cbe-148">Next, use these commands toocreate hello VNet-to-VNet VPN connection, which can take some time toocomplete.</span></span>

    $sharedKey="<pre-shared key value>"
    $gwTestLab=Get-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName
    $gwTestVNET=Get-AzureRmVirtualNetworkGateway -Name TestVNET_GW -ResourceGroupName $rgName
    New-AzureRmVirtualNetworkGatewayConnection -Name TestLab_to_TestVNET -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestLab -VirtualNetworkGateway2 $gwTestVNET -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey
    New-AzureRmVirtualNetworkGatewayConnection -Name TestVNET_to_TestLab -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestVNET -VirtualNetworkGateway2 $gwTestLab -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey

<span data-ttu-id="56cbe-149">Po několika minutách by se mělo vytvořit hello připojení.</span><span class="sxs-lookup"><span data-stu-id="56cbe-149">After a few minutes, hello connection should be established.</span></span>

<span data-ttu-id="56cbe-150">Toto je aktuální konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="56cbe-150">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph3.png)

## <a name="phase-4-configure-dc2"></a><span data-ttu-id="56cbe-151">Fáze 4: Konfigurace počítače DC2</span><span class="sxs-lookup"><span data-stu-id="56cbe-151">Phase 4: Configure DC2</span></span>
<span data-ttu-id="56cbe-152">Nejprve vytvořte virtuálního počítače pro řadič domény DC2.</span><span class="sxs-lookup"><span data-stu-id="56cbe-152">First, create a virtual machine for DC2.</span></span> <span data-ttu-id="56cbe-153">Spuštění těchto příkazů na příkazovém řádku prostředí Azure PowerShell hello v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="56cbe-153">Run these commands at hello Azure PowerShell command prompt on your local computer.</span></span>

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<hello storage account name for hello base configuration>"
    $vnet=Get-AzureRMVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $pip=New-AzureRMPublicIpAddress -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -PrivateIpAddress 192.168.0.4
    $vm=New-AzureRMVMConfig -VMName DC2 -VMSize Standard_A1
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestVNET-ADDSDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name ADDS-Data -DiskSizeInGB 20 -VhdUri $vhdURI  -CreateOption empty
    $cred=Get-Credential -Message "Type hello name and password of hello local administrator account for DC2."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName DC2 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name DC2-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

<span data-ttu-id="56cbe-154">V dalším kroku připojte toohello nový řadič domény DC2 virtuální počítač z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="56cbe-154">Next, connect toohello new DC2 virtual machine from hello Azure portal.</span></span>

<span data-ttu-id="56cbe-155">V dalším kroku nakonfigurujte přenosem tooallow pravidla brány Windows Firewall pro testování základní připojení.</span><span class="sxs-lookup"><span data-stu-id="56cbe-155">Next, configure a Windows Firewall rule tooallow traffic for basic connectivity testing.</span></span> <span data-ttu-id="56cbe-156">Z prostředí Windows PowerShell na úrovni správce příkazového řádku na počítači DC2 spuštění těchto příkazů.</span><span class="sxs-lookup"><span data-stu-id="56cbe-156">From an administrator-level Windows PowerShell command prompt on DC2, run these commands.</span></span>

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc1.corp.contoso.com

<span data-ttu-id="56cbe-157">příkaz ping Hello by měl mít za následek čtyři úspěšné odpovědi z IP adresy 10.0.0.4.</span><span class="sxs-lookup"><span data-stu-id="56cbe-157">hello ping command should result in four successful replies from IP address 10.0.0.4.</span></span> <span data-ttu-id="56cbe-158">Toto je test přenosů mezi hello připojení VNet-to-VNet.</span><span class="sxs-lookup"><span data-stu-id="56cbe-158">This is a test of traffic across hello VNet-to-VNet connection.</span></span>

<span data-ttu-id="56cbe-159">Dál přidejte hello navíc datový disk na počítači DC2 jako nový svazek s písmenem jednotky hello F:.</span><span class="sxs-lookup"><span data-stu-id="56cbe-159">Next, add hello extra data disk on DC2 as a new volume with hello drive letter F:.</span></span>

1. <span data-ttu-id="56cbe-160">V levém podokně hello správce serveru, klikněte na **Souborová služba a služba úložiště**a potom klikněte na **disky**.</span><span class="sxs-lookup"><span data-stu-id="56cbe-160">In hello left pane of Server Manager, click **File and Storage Services**, and then click **Disks**.</span></span>
2. <span data-ttu-id="56cbe-161">V podokně obsahu hello v hello **disky** klikněte na možnost **disku 2** (s hello **oddílu** nastavit příliš**neznámé**).</span><span class="sxs-lookup"><span data-stu-id="56cbe-161">In hello contents pane, in hello **Disks** group, click **disk 2** (with hello **Partition** set too**Unknown**).</span></span>
3. <span data-ttu-id="56cbe-162">Klikněte na tlačítko **úlohy**a potom klikněte na **nový svazek**.</span><span class="sxs-lookup"><span data-stu-id="56cbe-162">Click **Tasks**, and then click **New Volume**.</span></span>
4. <span data-ttu-id="56cbe-163">Na hello před zahájením stránku hello Průvodce vytvořením svazku, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="56cbe-163">On hello Before you begin page of hello New Volume Wizard, click **Next**.</span></span>
5. <span data-ttu-id="56cbe-164">Na serveru vyberte hello hello a stránka disku, klikněte na tlačítko **disku 2**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="56cbe-164">On hello Select hello server and disk page, click **Disk 2**, and then click **Next**.</span></span> <span data-ttu-id="56cbe-165">Po zobrazení výzvy klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="56cbe-165">When prompted, click **OK**.</span></span>
6. <span data-ttu-id="56cbe-166">Na hello zadat velikost hello hello svazku stránky, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="56cbe-166">On hello Specify hello size of hello volume page, click **Next**.</span></span>
7. <span data-ttu-id="56cbe-167">Na hello přiřazovat tooa jednotky písmeno nebo složka stránky, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="56cbe-167">On hello Assign tooa drive letter or folder page, click **Next**.</span></span>
8. <span data-ttu-id="56cbe-168">Na stránce nastavení systému hello vybrat soubor, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="56cbe-168">On hello Select file system settings page, click **Next**.</span></span>
9. <span data-ttu-id="56cbe-169">Na stránce Potvrdit vybrané možnosti hello klikněte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="56cbe-169">On hello Confirm selections page, click **Create**.</span></span>
10. <span data-ttu-id="56cbe-170">Po dokončení klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="56cbe-170">When complete, click **Close**.</span></span>

<span data-ttu-id="56cbe-171">V dalším kroku nakonfigurujte DC2 jako repliku řadiče domény pro doménu hello corp.contoso.com.</span><span class="sxs-lookup"><span data-stu-id="56cbe-171">Next, configure DC2 as a replica domain controller for hello corp.contoso.com domain.</span></span> <span data-ttu-id="56cbe-172">Spusťte tyto příkazy z příkazového řádku prostředí Windows PowerShell hello na počítači DC2.</span><span class="sxs-lookup"><span data-stu-id="56cbe-172">Run these commands from hello Windows PowerShell command prompt on DC2.</span></span>

    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -Credential (Get-Credential CORP\User1) -DomainName "corp.contoso.com" -InstallDns:$true -DatabasePath "F:\NTDS" -LogPath "F:\Logs" -SysvolPath "F:\SYSVOL"

<span data-ttu-id="56cbe-173">Všimněte si, že jste výzvami toosupply obě hello CORP\User1 heslo a heslo Directory režimu obnovení služeb (DSRM) a toorestart DC2.</span><span class="sxs-lookup"><span data-stu-id="56cbe-173">Note that you are prompted toosupply both hello CORP\User1 password and a Directory Services Restore Mode (DSRM) password, and toorestart DC2.</span></span>

<span data-ttu-id="56cbe-174">Teď, když hello TestVNET virtuální sítě má svou vlastní server DNS (DC2), musíte nakonfigurovat hello TestVNET virtuální sítě toouse tento server DNS.</span><span class="sxs-lookup"><span data-stu-id="56cbe-174">Now that hello TestVNET virtual network has its own DNS server (DC2), you must configure hello TestVNET virtual network toouse this DNS server.</span></span>

1. <span data-ttu-id="56cbe-175">V levém podokně hello Dobrý den portálu Azure klikněte na ikonu hello virtuální sítě a pak klikněte na tlačítko **TestVNET**.</span><span class="sxs-lookup"><span data-stu-id="56cbe-175">In hello left pane of hello Azure portal, click hello virtual networks icon, and then click **TestVNET**.</span></span>
2. <span data-ttu-id="56cbe-176">Na hello **nastavení** , klikněte na **servery DNS**.</span><span class="sxs-lookup"><span data-stu-id="56cbe-176">On hello **Settings** tab, click **DNS servers**.</span></span>
3. <span data-ttu-id="56cbe-177">V **primárního serveru DNS**, typ **192.168.0.4** tooreplace 10.0.0.4.</span><span class="sxs-lookup"><span data-stu-id="56cbe-177">In **Primary DNS server**, type **192.168.0.4** tooreplace 10.0.0.4.</span></span>
4. <span data-ttu-id="56cbe-178">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="56cbe-178">Click **Save**.</span></span>

<span data-ttu-id="56cbe-179">Toto je aktuální konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="56cbe-179">This is your current configuration.</span></span> 

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

<span data-ttu-id="56cbe-180">Simulované hybridní cloudové prostředí je nyní připraven pro testování.</span><span class="sxs-lookup"><span data-stu-id="56cbe-180">Your simulated hybrid cloud environment is now ready for testing.</span></span>

## <a name="next-step"></a><span data-ttu-id="56cbe-181">Další krok</span><span class="sxs-lookup"><span data-stu-id="56cbe-181">Next step</span></span>
* <span data-ttu-id="56cbe-182">Nastavení [webové, řádek obchodní aplikace](ps-hybrid-cloud-test-env-lob.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) v tomto prostředí.</span><span class="sxs-lookup"><span data-stu-id="56cbe-182">Set up a [web-based, line of business application](ps-hybrid-cloud-test-env-lob.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) in this environment.</span></span>

