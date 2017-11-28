---
title: "Konfigurace připojení S2S VPN aktivní aktivní pro brány sítě VPN: Azure Resource Manager: prostředí PowerShell | Microsoft Docs"
description: "Tento článek vás provede procesem konfigurace aktivní aktivní připojení s Azure VPN Gateway pomocí Azure Resource Manageru a prostředí PowerShell."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 238cd9b3-f1ce-4341-b18e-7390935604fa
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2017
ms.author: yushwang
ms.openlocfilehash: a9f71b566ffdb163f95634835f64589a700d712f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="configure-active-active-s2s-vpn-connections-with-azure-vpn-gateways"></a><span data-ttu-id="98818-103">Konfigurace připojení S2S VPN aktivní aktivní službou Azure VPN Gateways</span><span class="sxs-lookup"><span data-stu-id="98818-103">Configure active-active S2S VPN connections with Azure VPN Gateways</span></span>

<span data-ttu-id="98818-104">Tento článek vás provede kroky k vytvoření aktivní aktivní mezi různými místy a připojení VNet-to-VNet pomocí modelu nasazení Resource Manager a prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="98818-104">This article walks you through the steps to create active-active cross-premises and VNet-to-VNet connections using the Resource Manager deployment model and PowerShell.</span></span>

## <a name="about-highly-available-cross-premises-connections"></a><span data-ttu-id="98818-105">Informace o připojeních mezi různými místy vysokou dostupností</span><span class="sxs-lookup"><span data-stu-id="98818-105">About Highly Available Cross-Premises Connections</span></span>
<span data-ttu-id="98818-106">K dosažení vysoké dostupnosti pro mezi různými místy a připojení VNet-to-VNet, doporučujeme nasadit více bran VPN a vytvořit více paralelních připojení mezi vaší sítí a Azure.</span><span class="sxs-lookup"><span data-stu-id="98818-106">To achieve high availability for cross-premises and VNet-to-VNet connectivity, you should deploy multiple VPN gateways and establish multiple parallel connections between your networks and Azure.</span></span> <span data-ttu-id="98818-107">Najdete v tématu [vysoce dostupné mezi různými místy a připojení VNet-to-VNet](vpn-gateway-highlyavailable.md) přehled možností připojení a topologii.</span><span class="sxs-lookup"><span data-stu-id="98818-107">Please see [Highly Available Cross-Premises and VNet-to-VNet Connectivity](vpn-gateway-highlyavailable.md) for an overview of connectivity options and topology.</span></span>

<span data-ttu-id="98818-108">Tento článek obsahuje pokyny k nastavení připojení VPN aktivní aktivní mezi různými místy a aktivní aktivní připojení mezi dvěma virtuálními sítěmi:</span><span class="sxs-lookup"><span data-stu-id="98818-108">This article provides the instructions to set up an active-active cross-premises VPN connection, and active-active connection between two virtual networks:</span></span>

* [<span data-ttu-id="98818-109">Část 1 – Vytvoření a konfiguraci brány Azure VPN v režimu aktivní aktivní</span><span class="sxs-lookup"><span data-stu-id="98818-109">Part 1 - Create and configure your Azure VPN gateway in active-active mode</span></span>](#aagateway)
* [<span data-ttu-id="98818-110">Část 2 – navázání připojení mezi různými místy aktivní aktivní</span><span class="sxs-lookup"><span data-stu-id="98818-110">Part 2 - Establish active-active cross-premises connections</span></span>](#aacrossprem)
* [<span data-ttu-id="98818-111">Část 3 – vytvoření aktivní aktivní připojení VNet-to-VNet</span><span class="sxs-lookup"><span data-stu-id="98818-111">Part 3 - Establish active-active VNet-to-VNet connections</span></span>](#aav2v)
* [<span data-ttu-id="98818-112">Část 4 - aktualizace mezi aktivní aktivní a aktivní pohotovostní existující bránu</span><span class="sxs-lookup"><span data-stu-id="98818-112">Part 4 - Update existing gateway between active-active and active-standby</span></span>](#aaupdate)

<span data-ttu-id="98818-113">Můžete kombinovat tyto společně k vytvoření složitějších s vysokou dostupností síťové topologie, která vyhovuje vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="98818-113">You can combine these together to build a more complex, highly available network topology that meets your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="98818-114">Upozorňujeme, že aktivní aktivní režim využívá jenom následující SKU:</span><span class="sxs-lookup"><span data-stu-id="98818-114">Please note that the active-active mode uses only the following SKUs:</span></span> 
  * <span data-ttu-id="98818-115">VpnGw1, VpnGw2, VpnGw3</span><span class="sxs-lookup"><span data-stu-id="98818-115">VpnGw1, VpnGw2, VpnGw3</span></span>
  * <span data-ttu-id="98818-116">HighPerformance (pro původní starší verze SKU)</span><span class="sxs-lookup"><span data-stu-id="98818-116">HighPerformance (for old legacy SKUs)</span></span>
> 
> 

## <span data-ttu-id="98818-117"><a name ="aagateway"></a>Část 1 – Vytvoření a konfigurace aktivní aktivní brány sítě VPN</span><span class="sxs-lookup"><span data-stu-id="98818-117"><a name ="aagateway"></a>Part 1 - Create and configure active-active VPN gateways</span></span>
<span data-ttu-id="98818-118">Následující kroky nakonfiguruje bránu Azure VPN v režimech aktivní aktivní.</span><span class="sxs-lookup"><span data-stu-id="98818-118">The following steps will configure your Azure VPN gateway in active-active modes.</span></span> <span data-ttu-id="98818-119">Hlavní rozdíly mezi bránami aktivní aktivní a aktivní pohotovostní:</span><span class="sxs-lookup"><span data-stu-id="98818-119">The key differences between the active-active and active-standby gateways:</span></span>

* <span data-ttu-id="98818-120">Je nutné vytvořit dvě konfigurace IP brány s dvě veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="98818-120">You need to create two Gateway IP configurations with two public IP addresses</span></span>
* <span data-ttu-id="98818-121">Je nutné nastavit příznak EnableActiveActiveFeature</span><span class="sxs-lookup"><span data-stu-id="98818-121">You need set the EnableActiveActiveFeature flag</span></span>
* <span data-ttu-id="98818-122">Skladová položka brány musí být VpnGw1 VpnGw2, VpnGw3 nebo HighPerformance (starší verze SKU).</span><span class="sxs-lookup"><span data-stu-id="98818-122">The gateway SKU must be VpnGw1, VpnGw2, VpnGw3, or HighPerformance (legacy SKU).</span></span>

<span data-ttu-id="98818-123">Ostatní vlastnosti jsou stejné jako aktivní aktivní brány.</span><span class="sxs-lookup"><span data-stu-id="98818-123">The other properties are the same as the non-active-active gateways.</span></span> 

### <a name="before-you-begin"></a><span data-ttu-id="98818-124">Než začnete</span><span class="sxs-lookup"><span data-stu-id="98818-124">Before you begin</span></span>
* <span data-ttu-id="98818-125">Ověřte, že máte předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="98818-125">Verify that you have an Azure subscription.</span></span> <span data-ttu-id="98818-126">Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="98818-126">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="98818-127">Budete potřebovat nainstalovat nejnovější verzi rutin prostředí PowerShell pro Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="98818-127">You'll need to install the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="98818-128">V tématu [Přehled prostředí Azure PowerShell](/powershell/azure/overview) pro další informace o instalaci rutin prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="98818-128">See [Overview of Azure PowerShell](/powershell/azure/overview) for more information about installing the PowerShell cmdlets.</span></span>

### <a name="step-1---create-and-configure-vnet1"></a><span data-ttu-id="98818-129">Krok 1 – Vytvoření a konfigurace VNet1</span><span class="sxs-lookup"><span data-stu-id="98818-129">Step 1 - Create and configure VNet1</span></span>
#### <a name="1-declare-your-variables"></a><span data-ttu-id="98818-130">1. Deklarace proměnných</span><span class="sxs-lookup"><span data-stu-id="98818-130">1. Declare your variables</span></span>
<span data-ttu-id="98818-131">Tento ukázkový postup zahájíme deklarací proměnných.</span><span class="sxs-lookup"><span data-stu-id="98818-131">For this exercise, we'll start by declaring our variables.</span></span> <span data-ttu-id="98818-132">V následujícím příkladu jsou proměnné deklarovány s použitím hodnot pro tento ukázkový postup.</span><span class="sxs-lookup"><span data-stu-id="98818-132">The example below declares the variables using the values for this exercise.</span></span> <span data-ttu-id="98818-133">Při konfiguraci pro ostrý provoz nezapomeňte nahradit hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="98818-133">Be sure to replace the values with your own when configuring for production.</span></span> <span data-ttu-id="98818-134">Tyto proměnné můžete použít, pokud procházíte kroky, abyste se seznámili s tímto typem konfigurace.</span><span class="sxs-lookup"><span data-stu-id="98818-134">You can use these variables if you are running through the steps to become familiar with this type of configuration.</span></span> <span data-ttu-id="98818-135">Upravte proměnné a pak je zkopírujte a vložte do konzoly prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="98818-135">Modify the variables, and then copy and paste into your PowerShell console.</span></span>

```powershell
$Sub1 = "Ross"
$RG1 = "TestAARG1"
$Location1 = "West US"
$VNetName1 = "TestVNet1"
$FESubName1 = "FrontEnd"
$BESubName1 = "Backend"
$GWSubName1 = "GatewaySubnet"
$VNetPrefix11 = "10.11.0.0/16"
$VNetPrefix12 = "10.12.0.0/16"
$FESubPrefix1 = "10.11.0.0/24"
$BESubPrefix1 = "10.12.0.0/24"
$GWSubPrefix1 = "10.12.255.0/27"
$VNet1ASN = 65010
$DNS1 = "8.8.8.8"
$GWName1 = "VNet1GW"
$GW1IPName1 = "VNet1GWIP1"
$GW1IPName2 = "VNet1GWIP2"
$GW1IPconf1 = "gw1ipconf1"
$GW1IPconf2 = "gw1ipconf2"
$Connection12 = "VNet1toVNet2"
$Connection151 = "VNet1toSite5_1"
$Connection152 = "VNet1toSite5_2"
```

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a><span data-ttu-id="98818-136">2. Připojení k vašemu předplatnému a vytvořte novou skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="98818-136">2. Connect to your subscription and create a new resource group</span></span>
<span data-ttu-id="98818-137">Ujistěte se, že jste přešli do režimu prostředí PowerShell, aby bylo možné používat rutiny Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="98818-137">Make sure you switch to PowerShell mode to use the Resource Manager cmdlets.</span></span> <span data-ttu-id="98818-138">Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="98818-138">For more information, see [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="98818-139">Otevřete konzolu prostředí PowerShell a připojte se ke svému účtu.</span><span class="sxs-lookup"><span data-stu-id="98818-139">Open your PowerShell console and connect to your account.</span></span> <span data-ttu-id="98818-140">Připojení vám usnadní následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="98818-140">Use the following sample to help you connect:</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a><span data-ttu-id="98818-141">3. Vytvoření virtuální sítě TestVNet1</span><span class="sxs-lookup"><span data-stu-id="98818-141">3. Create TestVNet1</span></span>
<span data-ttu-id="98818-142">Následující ukázka vytvoří virtuální síť s názvem TestVNet1 a tři podsítě: jednu s názvem GatewaySubnet, jednu s názvem FrontEnd a jednu s názvem BackEnd.</span><span class="sxs-lookup"><span data-stu-id="98818-142">The sample below creates a virtual network named TestVNet1 and three subnets, one called GatewaySubnet, one called FrontEnd, and one called Backend.</span></span> <span data-ttu-id="98818-143">Při nahrazování hodnot je důležité vždy přiřadit podsíti brány konkrétní název GatewaySubnet.</span><span class="sxs-lookup"><span data-stu-id="98818-143">When substituting values, it's important that you always name your gateway subnet specifically GatewaySubnet.</span></span> <span data-ttu-id="98818-144">Pokud použijete jiný název, vytvoření brány se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="98818-144">If you name it something else, your gateway creation will fail.</span></span>

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-active-active-mode"></a><span data-ttu-id="98818-145">Krok 2 – Vytvoření brány VPN pro virtuální síť TestVNet1 s režimem aktivní aktivní</span><span class="sxs-lookup"><span data-stu-id="98818-145">Step 2 - Create the VPN gateway for TestVNet1 with active-active mode</span></span>
#### <a name="1-create-the-public-ip-addresses-and-gateway-ip-configurations"></a><span data-ttu-id="98818-146">1. Vytvoření veřejné IP adresy a brány konfigurace protokolu IP</span><span class="sxs-lookup"><span data-stu-id="98818-146">1. Create the public IP addresses and gateway IP configurations</span></span>
<span data-ttu-id="98818-147">Požádat o dvě veřejné adresy IP, která bude přidělena pro bránu, kterou vytvoříte pro virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="98818-147">Request two public IP addresses to be allocated to the gateway you will create for your VNet.</span></span> <span data-ttu-id="98818-148">Budete také definujte podsíť a požadované konfigurace protokolu IP.</span><span class="sxs-lookup"><span data-stu-id="98818-148">You'll also define the subnet and IP configurations required.</span></span>

```powershell
$gw1pip1 = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$gw1pip2 = New-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
$gw1ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2
```

#### <a name="2-create-the-vpn-gateway-with-active-active-configuration"></a><span data-ttu-id="98818-149">2. Vytvoření brány sítě VPN v konfiguraci aktivní aktivní</span><span class="sxs-lookup"><span data-stu-id="98818-149">2. Create the VPN gateway with active-active configuration</span></span>
<span data-ttu-id="98818-150">Vytvořte bránu virtuální sítě pro virtuální síť TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="98818-150">Create the virtual network gateway for TestVNet1.</span></span> <span data-ttu-id="98818-151">Upozorňujeme, že existují dvě položky GatewayIpConfig a je nastaven příznak EnableActiveActiveFeature.</span><span class="sxs-lookup"><span data-stu-id="98818-151">Note that there are two GatewayIpConfig entries, and the EnableActiveActiveFeature flag is set.</span></span> <span data-ttu-id="98818-152">Vytvoření brány může nějakou dobu trvat (45 minut nebo déle).</span><span class="sxs-lookup"><span data-stu-id="98818-152">Creating a gateway can take a while (45 minutes or more to complete).</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet1ASN -EnableActiveActiveFeature -Debug
```

#### <a name="3-obtain-the-gateway-public-ip-addresses-and-the-bgp-peer-ip-address"></a><span data-ttu-id="98818-153">3. Získat veřejné IP adresy brány a IP adresy partnera BGP</span><span class="sxs-lookup"><span data-stu-id="98818-153">3. Obtain the gateway public IP addresses and the BGP Peer IP address</span></span>
<span data-ttu-id="98818-154">Po vytvoření brány, musíte získat adresu IP adresa partnera BGP na bráně Azure VPN.</span><span class="sxs-lookup"><span data-stu-id="98818-154">Once the gateway is created, you will need to obtain the BGP Peer IP address on the Azure VPN Gateway.</span></span> <span data-ttu-id="98818-155">Tato adresa je potřeba ke konfiguraci Azure VPN Gateway jako partnerské zařízení protokolu BGP pro vaše místní zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="98818-155">This address is needed to configure the Azure VPN Gateway as a BGP Peer for your on-premises VPN devices.</span></span>

```powershell
$gw1pip1 = Get-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
$gw1pip2 = Get-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
```

<span data-ttu-id="98818-156">Pomocí následující rutiny zobrazíte dvě veřejné IP adresy přidělené vaší brány sítě VPN a jejich odpovídající IP adresu partnera BGP pro každou instanci brány:</span><span class="sxs-lookup"><span data-stu-id="98818-156">Use the following cmdlets to show the two public IP addresses allocated for your VPN gateway, and their corresponding BGP Peer IP addresses for each gateway instance:</span></span>

```powershell

    PS D:\> $gw1pip1.IpAddress
    40.112.190.5

    PS D:\> $gw1pip2.IpAddress
    138.91.156.129

    PS D:\> $vnet1gw.BgpSettingsText
    {
      "Asn": 65010,
      "BgpPeeringAddress": "10.12.255.4,10.12.255.5",
      "PeerWeight": 0
    }
```

<span data-ttu-id="98818-157">Pořadí veřejné IP adresy pro instance brány a odpovídající adresy partnerského vztahu protokolu BGP jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="98818-157">The order of the public IP addresses for the gateway instances and the corresponding BGP Peering Addresses are the same.</span></span> <span data-ttu-id="98818-158">V tomto příkladu bude brána virtuálního počítače s veřejnou IP adresu z 40.112.190.5 používat 10.12.255.4 jako adresy partnerského vztahu protokolu BGP a brána s 138.91.156.129 bude používat 10.12.255.5.</span><span class="sxs-lookup"><span data-stu-id="98818-158">In this example, the gateway VM with public IP of 40.112.190.5 will use 10.12.255.4 as its BGP Peering Address, and the gateway with 138.91.156.129 will use 10.12.255.5.</span></span> <span data-ttu-id="98818-159">Tyto informace budete potřebovat při nastavování vaše na zařízení VPN místní připojení k bráně aktivní aktivní.</span><span class="sxs-lookup"><span data-stu-id="98818-159">This information is needed when you set up your on premises VPN devices connecting to the active-active gateway.</span></span> <span data-ttu-id="98818-160">Brána je znázorněno na obrázku níže všechny adresy:</span><span class="sxs-lookup"><span data-stu-id="98818-160">The gateway is shown in the diagram below with all addresses:</span></span>

![aktivní aktivní brány](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

<span data-ttu-id="98818-162">Po vytvoření brány můžete vytvořit aktivní aktivní mezi různými místy nebo připojení VNet-to-VNet tuto bránu.</span><span class="sxs-lookup"><span data-stu-id="98818-162">Once the gateway is created, you can use this gateway to establish active-active cross-premises or VNet-to-VNet connection.</span></span> <span data-ttu-id="98818-163">V následujících částech provede kroky k dokončení výkonu.</span><span class="sxs-lookup"><span data-stu-id="98818-163">The following sections will walk through the steps to complete the exercise.</span></span>

## <span data-ttu-id="98818-164"><a name ="aacrossprem"></a>Část 2 – Vytvoření připojení mezi různými místy aktivní aktivní</span><span class="sxs-lookup"><span data-stu-id="98818-164"><a name ="aacrossprem"></a>Part 2 - Establish an active-active cross-premises connection</span></span>
<span data-ttu-id="98818-165">Navázat připojení mezi různými místy, musíte vytvořit bránu místní sítě představují vaše místní zařízení VPN a připojení pro připojení k místní síťová brána Azure VPN gateway.</span><span class="sxs-lookup"><span data-stu-id="98818-165">To establish a cross-premises connection, you need to create a Local Network Gateway to represent your on-premises VPN device, and a Connection to connect the Azure VPN gateway with the local network gateway.</span></span> <span data-ttu-id="98818-166">V tomto příkladu se brána Azure VPN je v režimu aktivní aktivní.</span><span class="sxs-lookup"><span data-stu-id="98818-166">In this example, the Azure VPN gateway is in active-active mode.</span></span> <span data-ttu-id="98818-167">V důsledku toho budou obě instance brány Azure VPN i v případě, že existuje pouze jeden místní zařízení VPN (brány místní sítě) a jedno připojení k prostředku, vytvořit tunelů S2S VPN s místní zařízení.</span><span class="sxs-lookup"><span data-stu-id="98818-167">As a result, even though there is only one on-premises VPN device (local network gateway) and one connection resource, both Azure VPN gateway instances will establish S2S VPN tunnels with the on-premises device.</span></span>

<span data-ttu-id="98818-168">Než budete pokračovat, Zkontrolujte prosím, že jste dokončili [část 1](#aagateway) tohoto cvičení.</span><span class="sxs-lookup"><span data-stu-id="98818-168">Before proceeding, please make sure you have completed [Part 1](#aagateway) of this exercise.</span></span>

### <a name="step-1---create-and-configure-the-local-network-gateway"></a><span data-ttu-id="98818-169">Krok 1 – Vytvoření a konfigurace brány místní sítě</span><span class="sxs-lookup"><span data-stu-id="98818-169">Step 1 - Create and configure the local network gateway</span></span>
#### <a name="1-declare-your-variables"></a><span data-ttu-id="98818-170">1. Deklarace proměnných</span><span class="sxs-lookup"><span data-stu-id="98818-170">1. Declare your variables</span></span>
<span data-ttu-id="98818-171">Toto cvičení bude při vytváření konfigurace znázorněné v diagramu.</span><span class="sxs-lookup"><span data-stu-id="98818-171">This exercise will continue to build the configuration shown in the diagram.</span></span> <span data-ttu-id="98818-172">Nezapomeňte nahradit hodnoty těmi, které chcete použít pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="98818-172">Be sure to replace the values with the ones that you want to use for your configuration.</span></span>

```powershell
$RG5 = "TestAARG5"
$Location5 = "West US"
$LNGName51 = "Site5_1"
$LNGPrefix51 = "10.52.255.253/32"
$LNGIP51 = "131.107.72.22"
$LNGASN5 = 65050
$BGPPeerIP51 = "10.52.255.253"
```

<span data-ttu-id="98818-173">Několik věcí, které si uvědomit o parametrech brány místní sítě:</span><span class="sxs-lookup"><span data-stu-id="98818-173">A couple of things to note regarding the local network gateway parameters:</span></span>

* <span data-ttu-id="98818-174">Brána místní sítě může být ve stejné nebo jiné umístění a skupině prostředků jako brána sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="98818-174">The local network gateway can be in the same or different location and resource group as the VPN gateway.</span></span> <span data-ttu-id="98818-175">Tento příklad ukazuje, je v různých skupinách prostředků, ale ve stejné oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="98818-175">This example shows them in different resource groups but in the same Azure location.</span></span>
* <span data-ttu-id="98818-176">Pokud existuje jenom jeden místní zařízení VPN jako v příkladu nahoře, aktivní aktivní připojení můžete pracovat s nebo bez protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="98818-176">If there is only one on-premises VPN device as shown above, the active-active connection can work with or without BGP protocol.</span></span> <span data-ttu-id="98818-177">Tento příklad používá protokol BGP pro připojení mezi různými místy.</span><span class="sxs-lookup"><span data-stu-id="98818-177">This example uses BGP for the cross-premises connection.</span></span>
* <span data-ttu-id="98818-178">Pokud je protokol BGP povolený, předpony, které je třeba deklarovat pro bránu místní sítě je adresa hostitele vaší IP adresy partnera BGP v zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="98818-178">If BGP is enabled, the prefix you need to declare for the local network gateway is the host address of your BGP Peer IP address on your VPN device.</span></span> <span data-ttu-id="98818-179">V takovém případě je /32 předponu "10.52.255.253/32".</span><span class="sxs-lookup"><span data-stu-id="98818-179">In this case, it's a /32 prefix of "10.52.255.253/32".</span></span>
* <span data-ttu-id="98818-180">Připomínáme je nutné použít různá čísla ASN protokolu BGP mezi vaší místní sítí a virtuální sítí VNet Azure.</span><span class="sxs-lookup"><span data-stu-id="98818-180">As a reminder, you must use different BGP ASNs between your on-premises networks and Azure VNet.</span></span> <span data-ttu-id="98818-181">Pokud se shodují, budete muset změnit číslo ASN vaší virtuální sítě, pokud vaše místní zařízení VPN už používá číslo Autonomního rovnocenných počítačů sousední ostatní směrovače BGP.</span><span class="sxs-lookup"><span data-stu-id="98818-181">If they are the same, you need to change your VNet ASN if your on-premises VPN device already uses the ASN to peer with other BGP neighbors.</span></span>

#### <a name="2-create-the-local-network-gateway-for-site5"></a><span data-ttu-id="98818-182">2. Vytvoření brány místní sítě pro Site5</span><span class="sxs-lookup"><span data-stu-id="98818-182">2. Create the local network gateway for Site5</span></span>
<span data-ttu-id="98818-183">Než budete pokračovat, zkontrolujte, že jste stále připojeni k předplatnému 1.</span><span class="sxs-lookup"><span data-stu-id="98818-183">Before you continue, please make sure you are still connected to Subscription 1.</span></span> <span data-ttu-id="98818-184">Vytvořte skupinu prostředků, pokud ještě není vytvořená.</span><span class="sxs-lookup"><span data-stu-id="98818-184">Create the resource group if it is not yet created.</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG5 -Location $Location5
New-AzureRmLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51
```

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a><span data-ttu-id="98818-185">Krok 2 – připojení brány virtuální sítě a brány místní sítě</span><span class="sxs-lookup"><span data-stu-id="98818-185">Step 2 - Connect the VNet gateway and local network gateway</span></span>
#### <a name="1-get-the-two-gateways"></a><span data-ttu-id="98818-186">1. Získat dvě brány</span><span class="sxs-lookup"><span data-stu-id="98818-186">1. Get the two gateways</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw1 = Get-AzureRmLocalNetworkGateway  -Name $LNGName51 -ResourceGroupName $RG5
```

#### <a name="2-create-the-testvnet1-to-site5-connection"></a><span data-ttu-id="98818-187">2. Vytvoření virtuální sítě TestVNet1 Site5 připojení</span><span class="sxs-lookup"><span data-stu-id="98818-187">2. Create the TestVNet1 to Site5 connection</span></span>
<span data-ttu-id="98818-188">V tomto kroku vytvoříte připojení z virtuální sítě TestVNet1 do Site5_1 s "EnableBGP" nastavena na $True.</span><span class="sxs-lookup"><span data-stu-id="98818-188">In this step, you will create the connection from TestVNet1 to Site5_1 with "EnableBGP" set to $True.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-on-premises-vpn-device"></a><span data-ttu-id="98818-189">3. Připojení VPN a BGP parametry pro vaše místní zařízení VPN</span><span class="sxs-lookup"><span data-stu-id="98818-189">3. VPN and BGP parameters for your on-premises VPN device</span></span>
<span data-ttu-id="98818-190">Následující příklad uvádí parametry, které budete zadávat do protokolu BGP konfigurační oddíl na vaše místní zařízení VPN pro tento postup:</span><span class="sxs-lookup"><span data-stu-id="98818-190">The example below lists the parameters you will enter into the BGP configuration section on your on-premises VPN device for this exercise:</span></span>

    - <span data-ttu-id="98818-191">Site5 číslo ASN: 65050</span><span class="sxs-lookup"><span data-stu-id="98818-191">Site5 ASN            : 65050</span></span>
    - <span data-ttu-id="98818-192">IP protokol BGP Site5: 10.52.255.253</span><span class="sxs-lookup"><span data-stu-id="98818-192">Site5 BGP IP         : 10.52.255.253</span></span>
    - <span data-ttu-id="98818-193">Předpony oznamujeme: (například) 10.51.0.0/16 a 10.52.0.0/16</span><span class="sxs-lookup"><span data-stu-id="98818-193">Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16</span></span>
    - <span data-ttu-id="98818-194">Azure VNet číslo ASN: 65010</span><span class="sxs-lookup"><span data-stu-id="98818-194">Azure VNet ASN       : 65010</span></span>
    - <span data-ttu-id="98818-195">Azure VNet IP protokolu BGP 1: 10.12.255.4 pro tunelové propojení k 40.112.190.5</span><span class="sxs-lookup"><span data-stu-id="98818-195">Azure VNet BGP IP 1  : 10.12.255.4 for tunnel to 40.112.190.5</span></span>
    - <span data-ttu-id="98818-196">Azure VNet IP protokolu BGP 2: 10.12.255.5 pro tunelové propojení k 138.91.156.129</span><span class="sxs-lookup"><span data-stu-id="98818-196">Azure VNet BGP IP 2  : 10.12.255.5 for tunnel to 138.91.156.129</span></span>
    - <span data-ttu-id="98818-197">Statické trasy: cílový 10.12.255.4/32, dalšího segmentu tunelového připojení sítě VPN rozhraní 40.112.190.5 cílové 10.12.255.5/32, rozhraní tunelového připojení sítě VPN k 138.91.156.129 dalšího segmentu</span><span class="sxs-lookup"><span data-stu-id="98818-197">Static routes        : Destination 10.12.255.4/32, nexthop the VPN tunnel interface to 40.112.190.5                        Destination 10.12.255.5/32, nexthop the VPN tunnel interface to 138.91.156.129</span></span>
    - <span data-ttu-id="98818-198">eBGP pokus: Zajistěte možnost "pokus" pro eBGP je povolena na vašem zařízení, v případě potřeby</span><span class="sxs-lookup"><span data-stu-id="98818-198">eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed</span></span>

<span data-ttu-id="98818-199">Připojení by se mělo vytvořit po několika minutách a relaci partnerského vztahu protokolu BGP se spustí po vytvoření připojení protokolu IPsec.</span><span class="sxs-lookup"><span data-stu-id="98818-199">The connection should be established after a few minutes, and the BGP peering session will start once the IPsec connection is established.</span></span> <span data-ttu-id="98818-200">Tento příklad, pokud byl nakonfigurován pouze jeden zařízení VPN místní, což vede k diagramu níže:</span><span class="sxs-lookup"><span data-stu-id="98818-200">This example so far has configured only one on-premises VPN device, resulting in the diagram shown below:</span></span>

![aktivní aktivní crossprem](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### <a name="step-3---connect-two-on-premises-vpn-devices-to-the-active-active-vpn-gateway"></a><span data-ttu-id="98818-202">Krok 3 – připojení dvě místní zařízení VPN k bráně VPN aktivní aktivní</span><span class="sxs-lookup"><span data-stu-id="98818-202">Step 3 - Connect two on-premises VPN devices to the active-active VPN gateway</span></span>
<span data-ttu-id="98818-203">Pokud máte dva zařízení VPN ve stejné místní síti, můžete dosáhnout duální redundance připojením bránu Azure VPN a druhý zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="98818-203">If you have two VPN devices at the same on-premises network, you can achieve dual redundancy by connecting the Azure VPN gateway to the second VPN device.</span></span>

#### <a name="1-create-the-second-local-network-gateway-for-site5"></a><span data-ttu-id="98818-204">1. Vytvořte druhý brány místní sítě pro Site5</span><span class="sxs-lookup"><span data-stu-id="98818-204">1. Create the second local network gateway for Site5</span></span>
<span data-ttu-id="98818-205">Všimněte si, že IP adresa brány, předponu adresy a adresu partnerského vztahu BGP pro bránu místní sítě druhý se nesmí překrývat s předchozí brána místní sítě pro stejnou místní síť.</span><span class="sxs-lookup"><span data-stu-id="98818-205">Note that the gateway IP address, address prefix, and BGP peering address for the second local network gateway must not overlap with the previous local network gateway for the same on-premises network.</span></span>

```powershell
$LNGName52 = "Site5_2"
$LNGPrefix52 = "10.52.255.254/32"
$LNGIP52 = "131.107.72.23"
$BGPPeerIP52 = "10.52.255.254"

New-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
```

#### <a name="2-connect-the-vnet-gateway-and-the-second-local-network-gateway"></a><span data-ttu-id="98818-206">2. Připojení brány virtuální sítě a druhá brána místní sítě</span><span class="sxs-lookup"><span data-stu-id="98818-206">2. Connect the VNet gateway and the second local network gateway</span></span>
<span data-ttu-id="98818-207">Vytvoření připojení z virtuální sítě TestVNet1 k Site5_2 s "EnableBGP" nastavena na $True</span><span class="sxs-lookup"><span data-stu-id="98818-207">Create the connection from TestVNet1 to Site5_2 with "EnableBGP" set to $True</span></span>

```powershell
$lng5gw2 = Get-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-second-on-premises-vpn-device"></a><span data-ttu-id="98818-208">3. Připojení VPN a BGP parametry pro druhý místní zařízení VPN</span><span class="sxs-lookup"><span data-stu-id="98818-208">3. VPN and BGP parameters for your second on-premises VPN device</span></span>
<span data-ttu-id="98818-209">Podobně níže seznamy parametry zadejte, který bude do druhé zařízení sítě VPN:</span><span class="sxs-lookup"><span data-stu-id="98818-209">Similarly, below lists the parameters you will enter into the second VPN device:</span></span>

```
- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP 1  : 10.12.255.4 for tunnel to 40.112.190.5
- Azure VNet BGP IP 2  : 10.12.255.5 for tunnel to 138.91.156.129
- Static routes        : Destination 10.12.255.4/32, nexthop the VPN tunnel interface to 40.112.190.5
                         Destination 10.12.255.5/32, nexthop the VPN tunnel interface to 138.91.156.129
- eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed
```

<span data-ttu-id="98818-210">Jakmile se navázat připojení (tunely), budete mít dva redundantní zařízení VPN a tunely připojení vaší místní sítí a Azure:</span><span class="sxs-lookup"><span data-stu-id="98818-210">Once the connection (tunnels) are established, you will have dual redundant VPN devices and tunnels connecting your on-premises network and Azure:</span></span>

![Dual redundance crossprem](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)

## <span data-ttu-id="98818-212"><a name ="aav2v"></a>Část 3 – vytvoření aktivní aktivní připojení VNet-to-VNet</span><span class="sxs-lookup"><span data-stu-id="98818-212"><a name ="aav2v"></a>Part 3 - Establish an active-active VNet-to-VNet connection</span></span>
<span data-ttu-id="98818-213">V této části vytvoříme připojení k aktivní aktivní VNet-to-VNet s protokolem BGP.</span><span class="sxs-lookup"><span data-stu-id="98818-213">This section creates an active-active VNet-to-VNet connection with BGP.</span></span> 

<span data-ttu-id="98818-214">Následující pokyny jsou pokračování kroků uvedených výše.</span><span class="sxs-lookup"><span data-stu-id="98818-214">The instructions below continue from the previous steps listed above.</span></span> <span data-ttu-id="98818-215">Je třeba provést [část 1](#aagateway) vytvořit a nakonfigurovat virtuální síť TestVNet1 a bránu VPN s protokolem BGP.</span><span class="sxs-lookup"><span data-stu-id="98818-215">You must complete [Part 1](#aagateway) to create and configure TestVNet1 and the VPN Gateway with BGP.</span></span> 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a><span data-ttu-id="98818-216">Krok 1 – Vytvoření TestVNet2 a brány VPN</span><span class="sxs-lookup"><span data-stu-id="98818-216">Step 1 - Create TestVNet2 and the VPN gateway</span></span>
<span data-ttu-id="98818-217">Je důležité se ujistit, že adresní prostor IP adres nové virtuální sítě, TestVNet2, nepřekrývá s žádným z rozsahů vaší virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="98818-217">It is important to make sure that the IP address space of the new virtual network, TestVNet2, does not overlap with any of your VNet ranges.</span></span>

<span data-ttu-id="98818-218">V tomto příkladu patří virtuální sítě ve stejném předplatném.</span><span class="sxs-lookup"><span data-stu-id="98818-218">In this example, the virtual networks belong to the same subscription.</span></span> <span data-ttu-id="98818-219">Můžete nastavit připojení VNet-to-VNet mezi různých předplatných; naleznete [konfigurace připojení typu VNet-to-VNet](vpn-gateway-vnet-vnet-rm-ps.md) podrobné informace.</span><span class="sxs-lookup"><span data-stu-id="98818-219">You can set up VNet-to-VNet connections between different subscriptions; please refer to [Configure a VNet-to-VNet connection](vpn-gateway-vnet-vnet-rm-ps.md) to learn more details.</span></span> <span data-ttu-id="98818-220">Zajistěte, aby přidáte "-EnableBgp $True" při vytváření připojení k povolení protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="98818-220">Make sure you add the "-EnableBgp $True" when creating the connections to enable BGP.</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="98818-221">1. Deklarace proměnných</span><span class="sxs-lookup"><span data-stu-id="98818-221">1. Declare your variables</span></span>
<span data-ttu-id="98818-222">Nezapomeňte nahradit hodnoty těmi, které chcete použít pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="98818-222">Be sure to replace the values with the ones that you want to use for your configuration.</span></span>

```powershell
$RG2 = "TestAARG2"
$Location2 = "East US"
$VNetName2 = "TestVNet2"
$FESubName2 = "FrontEnd"
$BESubName2 = "Backend"
$GWSubName2 = "GatewaySubnet"
$VNetPrefix21 = "10.21.0.0/16"
$VNetPrefix22 = "10.22.0.0/16"
$FESubPrefix2 = "10.21.0.0/24"
$BESubPrefix2 = "10.22.0.0/24"
$GWSubPrefix2 = "10.22.255.0/27"
$VNet2ASN = 65020
$DNS2 = "8.8.8.8"
$GWName2 = "VNet2GW"
$GW2IPName1 = "VNet2GWIP1"
$GW2IPconf1 = "gw2ipconf1"
$GW2IPName2 = "VNet2GWIP2"
$GW2IPconf2 = "gw2ipconf2"
$Connection21 = "VNet2toVNet1"
$Connection12 = "VNet1toVNet2"
```

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a><span data-ttu-id="98818-223">2. Vytvoření TestVNet2 do nové skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="98818-223">2. Create TestVNet2 in the new resource group</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2
```

#### <a name="3-create-the-active-active-vpn-gateway-for-testvnet2"></a><span data-ttu-id="98818-224">3. Vytvoření brány VPN aktivní aktivní pro TestVNet2</span><span class="sxs-lookup"><span data-stu-id="98818-224">3. Create the active-active VPN gateway for TestVNet2</span></span>
<span data-ttu-id="98818-225">Požádat o dvě veřejné adresy IP, která bude přidělena pro bránu, kterou vytvoříte pro virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="98818-225">Request two public IP addresses to be allocated to the gateway you will create for your VNet.</span></span> <span data-ttu-id="98818-226">Budete také definujte podsíť a požadované konfigurace protokolu IP.</span><span class="sxs-lookup"><span data-stu-id="98818-226">You'll also define the subnet and IP configurations required.</span></span>

```powershell
$gw2pip1 = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
$gw2pip2 = New-AzureRmPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2 = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
$gw2ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2
```

<span data-ttu-id="98818-227">Vytvoření brány VPN s čísla AS a příznak "EnableActiveActiveFeature".</span><span class="sxs-lookup"><span data-stu-id="98818-227">Create the VPN gateway with the AS number and the "EnableActiveActiveFeature" flag.</span></span> <span data-ttu-id="98818-228">Všimněte si, že je nutné přepsat výchozí číslo ASN na Azure VPN Gateway.</span><span class="sxs-lookup"><span data-stu-id="98818-228">Note that you must override the default ASN on your Azure VPN gateways.</span></span> <span data-ttu-id="98818-229">Čísla ASN pro připojené virtuální sítě se musí lišit povolit protokol BGP a směrování přenosu.</span><span class="sxs-lookup"><span data-stu-id="98818-229">The ASNs for the connected VNets must be different to enable BGP and transit routing.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet2ASN -EnableActiveActiveFeature
```

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a><span data-ttu-id="98818-230">Krok 2 – připojení brány virtuální sítě TestVNet1 a TestVNet2</span><span class="sxs-lookup"><span data-stu-id="98818-230">Step 2 - Connect the TestVNet1 and TestVNet2 gateways</span></span>
<span data-ttu-id="98818-231">V tomto příkladu jsou obě brány ve stejném předplatném.</span><span class="sxs-lookup"><span data-stu-id="98818-231">In this example, both gateways are in the same subscription.</span></span> <span data-ttu-id="98818-232">Tento krok můžete provést ve stejné relaci prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="98818-232">You can complete this step in the same PowerShell session.</span></span>

#### <a name="1-get-both-gateways"></a><span data-ttu-id="98818-233">1. Získat obě brány</span><span class="sxs-lookup"><span data-stu-id="98818-233">1. Get both gateways</span></span>
<span data-ttu-id="98818-234">Ujistěte se, že jste přihlášeni a připojeni k předplatnému 1.</span><span class="sxs-lookup"><span data-stu-id="98818-234">Make sure you log in and connect to Subscription 1.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
```

#### <a name="2-create-both-connections"></a><span data-ttu-id="98818-235">2. Vytvoření obě připojení</span><span class="sxs-lookup"><span data-stu-id="98818-235">2. Create both connections</span></span>
<span data-ttu-id="98818-236">V tomto kroku vytvoříte připojení z virtuální sítě TestVNet1 k TestVNet2 a připojení z TestVNet2 do virtuální sítě TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="98818-236">In this step, you will create the connection from TestVNet1 to TestVNet2, and the connection from TestVNet2 to TestVNet1.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> <span data-ttu-id="98818-237">Je nutné povolit protokol BGP pro obě připojení.</span><span class="sxs-lookup"><span data-stu-id="98818-237">Be sure to enable BGP for BOTH connections.</span></span>
> 
> 

<span data-ttu-id="98818-238">Po dokončení těchto kroků, připojení vytvoří pár minut a protokolu BGP bude relaci partnerského vztahu až po dokončení připojení VNet-to-VNet s duální redundance:</span><span class="sxs-lookup"><span data-stu-id="98818-238">After completing these steps, the connection will be establish in a few minutes, and the BGP peering session will be up once the VNet-to-VNet connection is completed with dual redundancy:</span></span>

![aktivní aktivní v2v](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <span data-ttu-id="98818-240"><a name ="aaupdate"></a>Část 4 - aktualizace mezi aktivní aktivní a aktivní pohotovostní existující bránu</span><span class="sxs-lookup"><span data-stu-id="98818-240"><a name ="aaupdate"></a>Part 4 - Update existing gateway between active-active and active-standby</span></span>
<span data-ttu-id="98818-241">V poslední části se popisují, jak můžete nakonfigurovat existující bránu Azure VPN z aktivní pohotovostní režim aktivní aktivní, nebo naopak.</span><span class="sxs-lookup"><span data-stu-id="98818-241">The last section will describe how you can configure an existing Azure VPN gateway from active-standby to active-active mode, or vice versa.</span></span>

> [!NOTE]
> <span data-ttu-id="98818-242">Tato část obsahuje kroky ke změně velikosti starší verze skladové jednotky (SKU staré) již vytvořené brány VPN ze standardní k HighPerformance.</span><span class="sxs-lookup"><span data-stu-id="98818-242">This section includes the steps to resize a legacy SKU (old SKU) of an already created VPN gateway from Standard to HighPerformance.</span></span> <span data-ttu-id="98818-243">Tyto kroky neupgradovat staré starší verze skladová položka na jednu z nové SKU.</span><span class="sxs-lookup"><span data-stu-id="98818-243">These steps do not upgrade an old legacy SKU to one of the new SKUs.</span></span>
> 
> 

### <a name="configure-an-active-standby-gateway-to-active-active-gateway"></a><span data-ttu-id="98818-244">Konfiguraci aktivní pohotovostní brány pro aktivní aktivní brány</span><span class="sxs-lookup"><span data-stu-id="98818-244">Configure an active-standby gateway to active-active gateway</span></span>
#### <a name="1-gateway-parameters"></a><span data-ttu-id="98818-245">1. Parametry brány</span><span class="sxs-lookup"><span data-stu-id="98818-245">1. Gateway parameters</span></span>
<span data-ttu-id="98818-246">Následující příklad převede bránu aktivní pohotovostní bránu aktivní aktivní.</span><span class="sxs-lookup"><span data-stu-id="98818-246">The following example converts an active-standby gateway into an active-active gateway.</span></span> <span data-ttu-id="98818-247">Budete muset vytvořit jinou veřejnou IP adresu a pak přidejte druhý konfigurace IP brány.</span><span class="sxs-lookup"><span data-stu-id="98818-247">You need to create another public IP address, then add a second Gateway IP configuration.</span></span> <span data-ttu-id="98818-248">Níže jsou uvedeny parametry použít:</span><span class="sxs-lookup"><span data-stu-id="98818-248">Below shows the parameters used:</span></span>

```powershell
$GWName = "TestVNetAA1GW"
$VNetName = "TestVNetAA1"
$RG = "TestVPNActiveActive01"
$GWIPName2 = "gwpip2"
$GWIPconf2 = "gw1ipconf2"

$vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
$location = $gw.Location
```

#### <a name="2-create-the-public-ip-address-then-add-the-second-gateway-ip-configuration"></a><span data-ttu-id="98818-249">2. Vytvoření veřejné IP adresy a pak přidejte druhý konfigurace IP brány</span><span class="sxs-lookup"><span data-stu-id="98818-249">2. Create the public IP address, then add the second gateway IP configuration</span></span>

```powershell
$gwpip2 = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
Add-AzureRmVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2
```

#### <a name="3-enable-active-active-mode-and-update-the-gateway"></a><span data-ttu-id="98818-250">3. Povolit režim aktivní aktivní a aktualizujte bránu</span><span class="sxs-lookup"><span data-stu-id="98818-250">3. Enable active-active mode and update the gateway</span></span>
<span data-ttu-id="98818-251">Objekt brány musí být nastavena v prostředí PowerShell pro aktivaci skutečné aktualizace.</span><span class="sxs-lookup"><span data-stu-id="98818-251">You must set the gateway object in PowerShell to trigger the actual update.</span></span> <span data-ttu-id="98818-252">Skladová položka brány virtuální sítě musí také změnit (změněnou) tak, aby HighPerformance vzhledem k tomu, že byla dříve vytvořena jako Standard.</span><span class="sxs-lookup"><span data-stu-id="98818-252">The SKU of the virtual network gateway must also be changed (resized) to HighPerformance since it was created previously as Standard.</span></span>

```powershell
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature -GatewaySku HighPerformance
```

<span data-ttu-id="98818-253">Tato aktualizace může trvat 30 až 45 minut.</span><span class="sxs-lookup"><span data-stu-id="98818-253">This update can take 30 to 45 minutes.</span></span>

### <a name="configure-an-active-active-gateway-to-active-standby-gateway"></a><span data-ttu-id="98818-254">Konfiguraci aktivní aktivní brány pro aktivní pohotovostní brány</span><span class="sxs-lookup"><span data-stu-id="98818-254">Configure an active-active gateway to active-standby gateway</span></span>
#### <a name="1-gateway-parameters"></a><span data-ttu-id="98818-255">1. Parametry brány</span><span class="sxs-lookup"><span data-stu-id="98818-255">1. Gateway parameters</span></span>
<span data-ttu-id="98818-256">Použít stejnými parametry, jak je uvedeno výše, získat název konfigurace protokolu IP, kterou chcete odebrat.</span><span class="sxs-lookup"><span data-stu-id="98818-256">Use the same parameters as above, get the name of the IP configuration you want to remove.</span></span>

```powershell
$GWName = "TestVNetAA1GW"
$RG = "TestVPNActiveActive01"

$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
$ipconfname = $gw.IpConfigurations[1].Name
```

#### <a name="2-remove-the-gateway-ip-configuration-and-disable-the-active-active-mode"></a><span data-ttu-id="98818-257">2. Odeberte konfigurace IP brány a zakázat režim aktivní aktivní</span><span class="sxs-lookup"><span data-stu-id="98818-257">2. Remove the gateway IP configuration and disable the active-active mode</span></span>
<span data-ttu-id="98818-258">Podobně je nutné nastavit objekt brány v prostředí PowerShell pro aktivaci skutečné aktualizace.</span><span class="sxs-lookup"><span data-stu-id="98818-258">Similarly, you must set the gateway object in PowerShell to trigger the actual update.</span></span>

```powershell
Remove-AzureRmVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature
```

<span data-ttu-id="98818-259">Tato aktualizace může trvat až 30 až 45 minut.</span><span class="sxs-lookup"><span data-stu-id="98818-259">This update can take up to 30 to  45 minutes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="98818-260">Další kroky</span><span class="sxs-lookup"><span data-stu-id="98818-260">Next steps</span></span>
<span data-ttu-id="98818-261">Po dokončení připojení můžete do virtuálních sítí přidávat virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="98818-261">Once your connection is complete, you can add virtual machines to your virtual networks.</span></span> <span data-ttu-id="98818-262">Kroky jsou uvedeny v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="98818-262">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>