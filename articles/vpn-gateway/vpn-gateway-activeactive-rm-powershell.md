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
ms.openlocfilehash: 964eedc7698e42bf0e082f0105845f2a339daf57
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-active-active-s2s-vpn-connections-with-azure-vpn-gateways"></a><span data-ttu-id="32bb2-103">Konfigurace připojení S2S VPN aktivní aktivní službou Azure VPN Gateways</span><span class="sxs-lookup"><span data-stu-id="32bb2-103">Configure active-active S2S VPN connections with Azure VPN Gateways</span></span>

<span data-ttu-id="32bb2-104">Tento článek vás provede kroky hello toocreate aktivní aktivní mezi různými místy a připojení VNet-to-VNet pomocí modelu nasazení Resource Manager hello a prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="32bb2-104">This article walks you through hello steps toocreate active-active cross-premises and VNet-to-VNet connections using hello Resource Manager deployment model and PowerShell.</span></span>

## <a name="about-highly-available-cross-premises-connections"></a><span data-ttu-id="32bb2-105">Informace o připojeních mezi různými místy vysokou dostupností</span><span class="sxs-lookup"><span data-stu-id="32bb2-105">About Highly Available Cross-Premises Connections</span></span>
<span data-ttu-id="32bb2-106">Vysoká dostupnost tooachieve mezi různými místy a připojení VNet-to-VNet, doporučujeme nasadit více bran VPN a vytvořit více paralelních připojení mezi vaší sítí a Azure.</span><span class="sxs-lookup"><span data-stu-id="32bb2-106">tooachieve high availability for cross-premises and VNet-to-VNet connectivity, you should deploy multiple VPN gateways and establish multiple parallel connections between your networks and Azure.</span></span> <span data-ttu-id="32bb2-107">Najdete v tématu [vysoce dostupné mezi různými místy a připojení VNet-to-VNet](vpn-gateway-highlyavailable.md) přehled možností připojení a topologii.</span><span class="sxs-lookup"><span data-stu-id="32bb2-107">Please see [Highly Available Cross-Premises and VNet-to-VNet Connectivity](vpn-gateway-highlyavailable.md) for an overview of connectivity options and topology.</span></span>

<span data-ttu-id="32bb2-108">Tento článek obsahuje pokyny hello tooset až aktivní aktivní mezi různými místy, připojení VPN a aktivní aktivní připojení mezi dvěma virtuálními sítěmi:</span><span class="sxs-lookup"><span data-stu-id="32bb2-108">This article provides hello instructions tooset up an active-active cross-premises VPN connection, and active-active connection between two virtual networks:</span></span>

* [<span data-ttu-id="32bb2-109">Část 1 – Vytvoření a konfiguraci brány Azure VPN v režimu aktivní aktivní</span><span class="sxs-lookup"><span data-stu-id="32bb2-109">Part 1 - Create and configure your Azure VPN gateway in active-active mode</span></span>](#aagateway)
* [<span data-ttu-id="32bb2-110">Část 2 – navázání připojení mezi různými místy aktivní aktivní</span><span class="sxs-lookup"><span data-stu-id="32bb2-110">Part 2 - Establish active-active cross-premises connections</span></span>](#aacrossprem)
* [<span data-ttu-id="32bb2-111">Část 3 – vytvoření aktivní aktivní připojení VNet-to-VNet</span><span class="sxs-lookup"><span data-stu-id="32bb2-111">Part 3 - Establish active-active VNet-to-VNet connections</span></span>](#aav2v)
* [<span data-ttu-id="32bb2-112">Část 4 - aktualizace mezi aktivní aktivní a aktivní pohotovostní existující bránu</span><span class="sxs-lookup"><span data-stu-id="32bb2-112">Part 4 - Update existing gateway between active-active and active-standby</span></span>](#aaupdate)

<span data-ttu-id="32bb2-113">Můžete kombinovat tyto společně toobuild složitější, vysoce dostupné síťové topologie, která vyhovuje vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="32bb2-113">You can combine these together toobuild a more complex, highly available network topology that meets your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="32bb2-114">Uvědomte si, že tento režim aktivní aktivní hello využívá jenom hello SKU následující:</span><span class="sxs-lookup"><span data-stu-id="32bb2-114">Please note that hello active-active mode uses only hello following SKUs:</span></span> 
  * <span data-ttu-id="32bb2-115">VpnGw1, VpnGw2, VpnGw3</span><span class="sxs-lookup"><span data-stu-id="32bb2-115">VpnGw1, VpnGw2, VpnGw3</span></span>
  * <span data-ttu-id="32bb2-116">HighPerformance (pro původní starší verze SKU)</span><span class="sxs-lookup"><span data-stu-id="32bb2-116">HighPerformance (for old legacy SKUs)</span></span>
> 
> 

## <span data-ttu-id="32bb2-117"><a name ="aagateway"></a>Část 1 – Vytvoření a konfigurace aktivní aktivní brány sítě VPN</span><span class="sxs-lookup"><span data-stu-id="32bb2-117"><a name ="aagateway"></a>Part 1 - Create and configure active-active VPN gateways</span></span>
<span data-ttu-id="32bb2-118">Následující kroky Hello nakonfiguruje bránu Azure VPN v režimech aktivní aktivní.</span><span class="sxs-lookup"><span data-stu-id="32bb2-118">hello following steps will configure your Azure VPN gateway in active-active modes.</span></span> <span data-ttu-id="32bb2-119">Hello hlavní rozdíly mezi bránami aktivní aktivní a aktivní pohotovostní hello:</span><span class="sxs-lookup"><span data-stu-id="32bb2-119">hello key differences between hello active-active and active-standby gateways:</span></span>

* <span data-ttu-id="32bb2-120">Je třeba toocreate dvě konfigurace IP brány s dvě veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="32bb2-120">You need toocreate two Gateway IP configurations with two public IP addresses</span></span>
* <span data-ttu-id="32bb2-121">Je nutné nastavit příznak EnableActiveActiveFeature hello</span><span class="sxs-lookup"><span data-stu-id="32bb2-121">You need set hello EnableActiveActiveFeature flag</span></span>
* <span data-ttu-id="32bb2-122">Skladová položka brány Hello musí být VpnGw1 VpnGw2, VpnGw3 nebo HighPerformance (starší verze SKU).</span><span class="sxs-lookup"><span data-stu-id="32bb2-122">hello gateway SKU must be VpnGw1, VpnGw2, VpnGw3, or HighPerformance (legacy SKU).</span></span>

<span data-ttu-id="32bb2-123">Hello ostatní vlastnosti jsou hello stejné jako hello aktivní aktivní brány.</span><span class="sxs-lookup"><span data-stu-id="32bb2-123">hello other properties are hello same as hello non-active-active gateways.</span></span> 

### <a name="before-you-begin"></a><span data-ttu-id="32bb2-124">Než začnete</span><span class="sxs-lookup"><span data-stu-id="32bb2-124">Before you begin</span></span>
* <span data-ttu-id="32bb2-125">Ověřte, že máte předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="32bb2-125">Verify that you have an Azure subscription.</span></span> <span data-ttu-id="32bb2-126">Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="32bb2-126">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="32bb2-127">Rutiny Powershellu pro Azure Resource Manager hello tooinstall budete potřebovat.</span><span class="sxs-lookup"><span data-stu-id="32bb2-127">You'll need tooinstall hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="32bb2-128">V tématu [Přehled prostředí Azure PowerShell](/powershell/azure/overview) Další informace o instalaci rutin prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="32bb2-128">See [Overview of Azure PowerShell](/powershell/azure/overview) for more information about installing hello PowerShell cmdlets.</span></span>

### <a name="step-1---create-and-configure-vnet1"></a><span data-ttu-id="32bb2-129">Krok 1 – Vytvoření a konfigurace VNet1</span><span class="sxs-lookup"><span data-stu-id="32bb2-129">Step 1 - Create and configure VNet1</span></span>
#### <a name="1-declare-your-variables"></a><span data-ttu-id="32bb2-130">1. Deklarace proměnných</span><span class="sxs-lookup"><span data-stu-id="32bb2-130">1. Declare your variables</span></span>
<span data-ttu-id="32bb2-131">Tento ukázkový postup zahájíme deklarací proměnných.</span><span class="sxs-lookup"><span data-stu-id="32bb2-131">For this exercise, we'll start by declaring our variables.</span></span> <span data-ttu-id="32bb2-132">Následující příklad Hello deklaruje hello proměnné pomocí hello hodnot pro toto cvičení.</span><span class="sxs-lookup"><span data-stu-id="32bb2-132">hello example below declares hello variables using hello values for this exercise.</span></span> <span data-ttu-id="32bb2-133">Zda tooreplace hello hodnoty vlastními být při konfiguraci pro produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="32bb2-133">Be sure tooreplace hello values with your own when configuring for production.</span></span> <span data-ttu-id="32bb2-134">Tyto proměnné můžete použít, pokud používáte prostřednictvím toobecome kroky hello seznámili s tímto typem konfigurace.</span><span class="sxs-lookup"><span data-stu-id="32bb2-134">You can use these variables if you are running through hello steps toobecome familiar with this type of configuration.</span></span> <span data-ttu-id="32bb2-135">Umožňuje změnit proměnné hello a pak zkopírujte a vložte do konzoly prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="32bb2-135">Modify hello variables, and then copy and paste into your PowerShell console.</span></span>

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

#### <a name="2-connect-tooyour-subscription-and-create-a-new-resource-group"></a><span data-ttu-id="32bb2-136">2. Připojení tooyour odběru a vytvořit novou skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="32bb2-136">2. Connect tooyour subscription and create a new resource group</span></span>
<span data-ttu-id="32bb2-137">Ujistěte se, že jste přešli tooPowerShell režimu toouse hello rutiny správce prostředků.</span><span class="sxs-lookup"><span data-stu-id="32bb2-137">Make sure you switch tooPowerShell mode toouse hello Resource Manager cmdlets.</span></span> <span data-ttu-id="32bb2-138">Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="32bb2-138">For more information, see [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="32bb2-139">Otevřete konzolu prostředí PowerShell a připojte tooyour účtu.</span><span class="sxs-lookup"><span data-stu-id="32bb2-139">Open your PowerShell console and connect tooyour account.</span></span> <span data-ttu-id="32bb2-140">Použijte následující ukázka toohelp, ke kterým se připojujete hello:</span><span class="sxs-lookup"><span data-stu-id="32bb2-140">Use hello following sample toohelp you connect:</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a><span data-ttu-id="32bb2-141">3. Vytvoření virtuální sítě TestVNet1</span><span class="sxs-lookup"><span data-stu-id="32bb2-141">3. Create TestVNet1</span></span>
<span data-ttu-id="32bb2-142">Hello následující ukázka vytvoří virtuální síť s názvem TestVNet1 a tři podsítě, jednu s názvem GatewaySubnet, jednu s názvem FrontEnd a jednu s názvem Backend.</span><span class="sxs-lookup"><span data-stu-id="32bb2-142">hello sample below creates a virtual network named TestVNet1 and three subnets, one called GatewaySubnet, one called FrontEnd, and one called Backend.</span></span> <span data-ttu-id="32bb2-143">Při nahrazování hodnot je důležité vždy přiřadit podsíti brány konkrétní název GatewaySubnet.</span><span class="sxs-lookup"><span data-stu-id="32bb2-143">When substituting values, it's important that you always name your gateway subnet specifically GatewaySubnet.</span></span> <span data-ttu-id="32bb2-144">Pokud použijete jiný název, vytvoření brány se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="32bb2-144">If you name it something else, your gateway creation will fail.</span></span>

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-hello-vpn-gateway-for-testvnet1-with-active-active-mode"></a><span data-ttu-id="32bb2-145">Krok 2 – Vytvoření brány VPN hello s režimem aktivní aktivní pro virtuální síť TestVNet1</span><span class="sxs-lookup"><span data-stu-id="32bb2-145">Step 2 - Create hello VPN gateway for TestVNet1 with active-active mode</span></span>
#### <a name="1-create-hello-public-ip-addresses-and-gateway-ip-configurations"></a><span data-ttu-id="32bb2-146">1. Vytvoření hello veřejné IP adresy a konfigurace protokolu IP brány</span><span class="sxs-lookup"><span data-stu-id="32bb2-146">1. Create hello public IP addresses and gateway IP configurations</span></span>
<span data-ttu-id="32bb2-147">Žádost o dvě veřejné IP adresy toobe přidělené toohello brány, které vytvoříte pro virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="32bb2-147">Request two public IP addresses toobe allocated toohello gateway you will create for your VNet.</span></span> <span data-ttu-id="32bb2-148">Budete také definovat hello podsíť a požadované konfigurace protokolu IP.</span><span class="sxs-lookup"><span data-stu-id="32bb2-148">You'll also define hello subnet and IP configurations required.</span></span>

```powershell
$gw1pip1 = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$gw1pip2 = New-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
$gw1ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2
```

#### <a name="2-create-hello-vpn-gateway-with-active-active-configuration"></a><span data-ttu-id="32bb2-149">2. Vytvoření brány VPN hello v konfiguraci aktivní aktivní</span><span class="sxs-lookup"><span data-stu-id="32bb2-149">2. Create hello VPN gateway with active-active configuration</span></span>
<span data-ttu-id="32bb2-150">Vytvoření brány virtuální sítě hello pro virtuální síť TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="32bb2-150">Create hello virtual network gateway for TestVNet1.</span></span> <span data-ttu-id="32bb2-151">Upozorňujeme, že existují dvě položky GatewayIpConfig a hello EnableActiveActiveFeature příznak nastaven.</span><span class="sxs-lookup"><span data-stu-id="32bb2-151">Note that there are two GatewayIpConfig entries, and hello EnableActiveActiveFeature flag is set.</span></span> <span data-ttu-id="32bb2-152">Vytvoření brány může chvíli trvat (45 minut nebo další toocomplete).</span><span class="sxs-lookup"><span data-stu-id="32bb2-152">Creating a gateway can take a while (45 minutes or more toocomplete).</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet1ASN -EnableActiveActiveFeature -Debug
```

#### <a name="3-obtain-hello-gateway-public-ip-addresses-and-hello-bgp-peer-ip-address"></a><span data-ttu-id="32bb2-153">3. Získat hello brány veřejné IP adresy a hello IP adresy partnera BGP</span><span class="sxs-lookup"><span data-stu-id="32bb2-153">3. Obtain hello gateway public IP addresses and hello BGP Peer IP address</span></span>
<span data-ttu-id="32bb2-154">Po vytvoření brány hello budete potřebovat tooobtain hello IP adresy partnera BGP na hello Azure VPN Gateway.</span><span class="sxs-lookup"><span data-stu-id="32bb2-154">Once hello gateway is created, you will need tooobtain hello BGP Peer IP address on hello Azure VPN Gateway.</span></span> <span data-ttu-id="32bb2-155">Tato adresa je potřebné tooconfigure hello Azure VPN Gateway jako partnerské zařízení protokolu BGP pro vaše místní zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="32bb2-155">This address is needed tooconfigure hello Azure VPN Gateway as a BGP Peer for your on-premises VPN devices.</span></span>

```powershell
$gw1pip1 = Get-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
$gw1pip2 = Get-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
```

<span data-ttu-id="32bb2-156">Pomocí následující rutiny tooshow hello dvě veřejné IP adresy přidělené vaší brány sítě VPN a jejich odpovídající IP adresu partnera BGP pro každou instanci brány hello:</span><span class="sxs-lookup"><span data-stu-id="32bb2-156">Use hello following cmdlets tooshow hello two public IP addresses allocated for your VPN gateway, and their corresponding BGP Peer IP addresses for each gateway instance:</span></span>

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

<span data-ttu-id="32bb2-157">Hello pořadí hello veřejné IP adresy pro instance brány hello a hello, které jsou adresy odpovídající partnerského vztahu protokolu BGP hello stejné.</span><span class="sxs-lookup"><span data-stu-id="32bb2-157">hello order of hello public IP addresses for hello gateway instances and hello corresponding BGP Peering Addresses are hello same.</span></span> <span data-ttu-id="32bb2-158">V tomto příkladu hello brány virtuálních počítačů s veřejnou IP adresu z 40.112.190.5 použije 10.12.255.4 jako adresy partnerského vztahu protokolu BGP a brána hello s 138.91.156.129 bude používat 10.12.255.5.</span><span class="sxs-lookup"><span data-stu-id="32bb2-158">In this example, hello gateway VM with public IP of 40.112.190.5 will use 10.12.255.4 as its BGP Peering Address, and hello gateway with 138.91.156.129 will use 10.12.255.5.</span></span> <span data-ttu-id="32bb2-159">Tyto informace budete potřebovat při nastavování vašeho místního zařízení VPN připojení brány toohello aktivní aktivní.</span><span class="sxs-lookup"><span data-stu-id="32bb2-159">This information is needed when you set up your on premises VPN devices connecting toohello active-active gateway.</span></span> <span data-ttu-id="32bb2-160">Hello brány je zobrazena v diagramu hello níže všechny adresy:</span><span class="sxs-lookup"><span data-stu-id="32bb2-160">hello gateway is shown in hello diagram below with all addresses:</span></span>

![aktivní aktivní brány](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

<span data-ttu-id="32bb2-162">Po vytvoření brány hello můžete použít tuto bránu tooestablish aktivní aktivní mezi různými místy nebo připojení VNet-to-VNet.</span><span class="sxs-lookup"><span data-stu-id="32bb2-162">Once hello gateway is created, you can use this gateway tooestablish active-active cross-premises or VNet-to-VNet connection.</span></span> <span data-ttu-id="32bb2-163">Následující části Hello provede hello kroky toocomplete hello cvičení.</span><span class="sxs-lookup"><span data-stu-id="32bb2-163">hello following sections will walk through hello steps toocomplete hello exercise.</span></span>

## <span data-ttu-id="32bb2-164"><a name ="aacrossprem"></a>Část 2 – Vytvoření připojení mezi různými místy aktivní aktivní</span><span class="sxs-lookup"><span data-stu-id="32bb2-164"><a name ="aacrossprem"></a>Part 2 - Establish an active-active cross-premises connection</span></span>
<span data-ttu-id="32bb2-165">tooestablish připojení mezi různými místy, budete potřebovat toocreate bránu místní sítě toorepresent vaše místní zařízení VPN a brány Azure VPN připojení tooconnect hello s hello brány místní sítě.</span><span class="sxs-lookup"><span data-stu-id="32bb2-165">tooestablish a cross-premises connection, you need toocreate a Local Network Gateway toorepresent your on-premises VPN device, and a Connection tooconnect hello Azure VPN gateway with hello local network gateway.</span></span> <span data-ttu-id="32bb2-166">V tomto příkladu hello Azure VPN gateway je v režimu aktivní aktivní.</span><span class="sxs-lookup"><span data-stu-id="32bb2-166">In this example, hello Azure VPN gateway is in active-active mode.</span></span> <span data-ttu-id="32bb2-167">V důsledku toho budou obě instance brány Azure VPN i v případě, že existuje pouze jeden místní zařízení VPN (brány místní sítě) a jedno připojení k prostředku, vytvořit tunelů S2S VPN s hello místního zařízení.</span><span class="sxs-lookup"><span data-stu-id="32bb2-167">As a result, even though there is only one on-premises VPN device (local network gateway) and one connection resource, both Azure VPN gateway instances will establish S2S VPN tunnels with hello on-premises device.</span></span>

<span data-ttu-id="32bb2-168">Než budete pokračovat, Zkontrolujte prosím, že jste dokončili [část 1](#aagateway) tohoto cvičení.</span><span class="sxs-lookup"><span data-stu-id="32bb2-168">Before proceeding, please make sure you have completed [Part 1](#aagateway) of this exercise.</span></span>

### <a name="step-1---create-and-configure-hello-local-network-gateway"></a><span data-ttu-id="32bb2-169">Krok 1 – Vytvoření a konfigurace brány místní sítě hello</span><span class="sxs-lookup"><span data-stu-id="32bb2-169">Step 1 - Create and configure hello local network gateway</span></span>
#### <a name="1-declare-your-variables"></a><span data-ttu-id="32bb2-170">1. Deklarace proměnných</span><span class="sxs-lookup"><span data-stu-id="32bb2-170">1. Declare your variables</span></span>
<span data-ttu-id="32bb2-171">Toto cvičení bude pokračovat v konfiguraci hello toobuild vidět v diagramu hello.</span><span class="sxs-lookup"><span data-stu-id="32bb2-171">This exercise will continue toobuild hello configuration shown in hello diagram.</span></span> <span data-ttu-id="32bb2-172">Být jisti tooreplace hello hodnoty hello ty, které jsou chcete toouse pro vaši konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="32bb2-172">Be sure tooreplace hello values with hello ones that you want toouse for your configuration.</span></span>

```powershell
$RG5 = "TestAARG5"
$Location5 = "West US"
$LNGName51 = "Site5_1"
$LNGPrefix51 = "10.52.255.253/32"
$LNGIP51 = "131.107.72.22"
$LNGASN5 = 65050
$BGPPeerIP51 = "10.52.255.253"
```

<span data-ttu-id="32bb2-173">Pár věcí toonote týkající se parametrů brány místní sítě hello:</span><span class="sxs-lookup"><span data-stu-id="32bb2-173">A couple of things toonote regarding hello local network gateway parameters:</span></span>

* <span data-ttu-id="32bb2-174">Brána místní sítě Hello může být v hello stejné nebo jiné umístění a prostředků skupiny jako hello brány VPN.</span><span class="sxs-lookup"><span data-stu-id="32bb2-174">hello local network gateway can be in hello same or different location and resource group as hello VPN gateway.</span></span> <span data-ttu-id="32bb2-175">Tento příklad ukazuje v různých skupinách prostředků, ale v hello stejného umístění Azure.</span><span class="sxs-lookup"><span data-stu-id="32bb2-175">This example shows them in different resource groups but in hello same Azure location.</span></span>
* <span data-ttu-id="32bb2-176">Pokud existuje jenom jeden místní zařízení VPN jako v příkladu nahoře, hello aktivní aktivní připojení můžete pracovat s nebo bez protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="32bb2-176">If there is only one on-premises VPN device as shown above, hello active-active connection can work with or without BGP protocol.</span></span> <span data-ttu-id="32bb2-177">Tento příklad používá protokol BGP pro připojení mezi různými místy hello.</span><span class="sxs-lookup"><span data-stu-id="32bb2-177">This example uses BGP for hello cross-premises connection.</span></span>
* <span data-ttu-id="32bb2-178">Pokud je protokol BGP povolený, předpona hello potřebujete toodeclare pro bránu místní sítě hello je adresa hostitele hello vaší IP adresy partnera BGP v zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="32bb2-178">If BGP is enabled, hello prefix you need toodeclare for hello local network gateway is hello host address of your BGP Peer IP address on your VPN device.</span></span> <span data-ttu-id="32bb2-179">V takovém případě je /32 předponu "10.52.255.253/32".</span><span class="sxs-lookup"><span data-stu-id="32bb2-179">In this case, it's a /32 prefix of "10.52.255.253/32".</span></span>
* <span data-ttu-id="32bb2-180">Připomínáme je nutné použít různá čísla ASN protokolu BGP mezi vaší místní sítí a virtuální sítí VNet Azure.</span><span class="sxs-lookup"><span data-stu-id="32bb2-180">As a reminder, you must use different BGP ASNs between your on-premises networks and Azure VNet.</span></span> <span data-ttu-id="32bb2-181">Pokud se jsou hello stejné, je třeba toochange vaší virtuální sítě ASN Pokud vaše místní zařízení VPN už používá hello ASN toopeer sousední ostatní směrovače BGP.</span><span class="sxs-lookup"><span data-stu-id="32bb2-181">If they are hello same, you need toochange your VNet ASN if your on-premises VPN device already uses hello ASN toopeer with other BGP neighbors.</span></span>

#### <a name="2-create-hello-local-network-gateway-for-site5"></a><span data-ttu-id="32bb2-182">2. Vytvoření brány místní sítě hello pro Site5</span><span class="sxs-lookup"><span data-stu-id="32bb2-182">2. Create hello local network gateway for Site5</span></span>
<span data-ttu-id="32bb2-183">Než budete pokračovat, Zkontrolujte prosím, že jsou stále připojená tooSubscription 1.</span><span class="sxs-lookup"><span data-stu-id="32bb2-183">Before you continue, please make sure you are still connected tooSubscription 1.</span></span> <span data-ttu-id="32bb2-184">Vytvořte skupinu prostředků hello, pokud ještě není vytvořená.</span><span class="sxs-lookup"><span data-stu-id="32bb2-184">Create hello resource group if it is not yet created.</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG5 -Location $Location5
New-AzureRmLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51
```

### <a name="step-2---connect-hello-vnet-gateway-and-local-network-gateway"></a><span data-ttu-id="32bb2-185">Krok 2 – připojit hello brány virtuální sítě a brány místní sítě</span><span class="sxs-lookup"><span data-stu-id="32bb2-185">Step 2 - Connect hello VNet gateway and local network gateway</span></span>
#### <a name="1-get-hello-two-gateways"></a><span data-ttu-id="32bb2-186">1. Získat hello dvě brány</span><span class="sxs-lookup"><span data-stu-id="32bb2-186">1. Get hello two gateways</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw1 = Get-AzureRmLocalNetworkGateway  -Name $LNGName51 -ResourceGroupName $RG5
```

#### <a name="2-create-hello-testvnet1-toosite5-connection"></a><span data-ttu-id="32bb2-187">2. Vytvoření připojení tooSite5 hello virtuální sítě TestVNet1</span><span class="sxs-lookup"><span data-stu-id="32bb2-187">2. Create hello TestVNet1 tooSite5 connection</span></span>
<span data-ttu-id="32bb2-188">V tomto kroku vytvoříte hello připojení z virtuální sítě TestVNet1 tooSite5_1 s "EnableBGP" nastavit také$ True.</span><span class="sxs-lookup"><span data-stu-id="32bb2-188">In this step, you will create hello connection from TestVNet1 tooSite5_1 with "EnableBGP" set too$True.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-on-premises-vpn-device"></a><span data-ttu-id="32bb2-189">3. Připojení VPN a BGP parametry pro vaše místní zařízení VPN</span><span class="sxs-lookup"><span data-stu-id="32bb2-189">3. VPN and BGP parameters for your on-premises VPN device</span></span>
<span data-ttu-id="32bb2-190">Následující příklad Hello uvádí hello parametry, které budete zadávat do hello BGP konfigurační oddíl na vaše místní zařízení VPN pro toto cvičení:</span><span class="sxs-lookup"><span data-stu-id="32bb2-190">hello example below lists hello parameters you will enter into hello BGP configuration section on your on-premises VPN device for this exercise:</span></span>

    - <span data-ttu-id="32bb2-191">Site5 číslo ASN: 65050</span><span class="sxs-lookup"><span data-stu-id="32bb2-191">Site5 ASN            : 65050</span></span>
    - <span data-ttu-id="32bb2-192">IP protokol BGP Site5: 10.52.255.253</span><span class="sxs-lookup"><span data-stu-id="32bb2-192">Site5 BGP IP         : 10.52.255.253</span></span>
    - <span data-ttu-id="32bb2-193">Předpony tooannounce: (například) 10.51.0.0/16 a 10.52.0.0/16</span><span class="sxs-lookup"><span data-stu-id="32bb2-193">Prefixes tooannounce : (for example) 10.51.0.0/16 and 10.52.0.0/16</span></span>
    - <span data-ttu-id="32bb2-194">Azure VNet číslo ASN: 65010</span><span class="sxs-lookup"><span data-stu-id="32bb2-194">Azure VNet ASN       : 65010</span></span>
    - <span data-ttu-id="32bb2-195">Azure VNet IP protokolu BGP 1: 10.12.255.4 pro too40.112.190.5 tunelového propojení</span><span class="sxs-lookup"><span data-stu-id="32bb2-195">Azure VNet BGP IP 1  : 10.12.255.4 for tunnel too40.112.190.5</span></span>
    - <span data-ttu-id="32bb2-196">Azure VNet IP protokolu BGP 2: 10.12.255.5 pro too138.91.156.129 tunelového propojení</span><span class="sxs-lookup"><span data-stu-id="32bb2-196">Azure VNet BGP IP 2  : 10.12.255.5 for tunnel too138.91.156.129</span></span>
    - <span data-ttu-id="32bb2-197">Statické trasy: cílový 10.12.255.4/32, dalšího segmentu hello VPN tunelového propojení rozhraní too40.112.190.5 cílové 10.12.255.5/32, dalšího segmentu hello VPN tunelového propojení rozhraní too138.91.156.129</span><span class="sxs-lookup"><span data-stu-id="32bb2-197">Static routes        : Destination 10.12.255.4/32, nexthop hello VPN tunnel interface too40.112.190.5                        Destination 10.12.255.5/32, nexthop hello VPN tunnel interface too138.91.156.129</span></span>
    - <span data-ttu-id="32bb2-198">eBGP pokus: Zajistěte možnost "pokus" hello, eBGP je povolena na vašem zařízení, v případě potřeby</span><span class="sxs-lookup"><span data-stu-id="32bb2-198">eBGP Multihop        : Ensure hello "multihop" option for eBGP is enabled on your device if needed</span></span>

<span data-ttu-id="32bb2-199">Hello připojení by se mělo vytvořit po několik minut a hello relaci partnerského vztahu protokolu BGP se spustí po vytvoření hello připojení protokolu IPsec.</span><span class="sxs-lookup"><span data-stu-id="32bb2-199">hello connection should be established after a few minutes, and hello BGP peering session will start once hello IPsec connection is established.</span></span> <span data-ttu-id="32bb2-200">Tento příklad, pokud byl nakonfigurován pouze jeden zařízení VPN místní, což vede k hello diagramu níže:</span><span class="sxs-lookup"><span data-stu-id="32bb2-200">This example so far has configured only one on-premises VPN device, resulting in hello diagram shown below:</span></span>

![aktivní aktivní crossprem](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### <a name="step-3---connect-two-on-premises-vpn-devices-toohello-active-active-vpn-gateway"></a><span data-ttu-id="32bb2-202">Krok 3 – připojení dvě místní sítě VPN zařízení toohello aktivní aktivní brány VPN</span><span class="sxs-lookup"><span data-stu-id="32bb2-202">Step 3 - Connect two on-premises VPN devices toohello active-active VPN gateway</span></span>
<span data-ttu-id="32bb2-203">Pokud máte dva zařízení VPN na hello stejné místní síti, můžete dosáhnout duální redundance připojování hello Azure VPN gateway toohello druhé zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="32bb2-203">If you have two VPN devices at hello same on-premises network, you can achieve dual redundancy by connecting hello Azure VPN gateway toohello second VPN device.</span></span>

#### <a name="1-create-hello-second-local-network-gateway-for-site5"></a><span data-ttu-id="32bb2-204">1. Vytvoření brány místní sítě druhý hello pro Site5</span><span class="sxs-lookup"><span data-stu-id="32bb2-204">1. Create hello second local network gateway for Site5</span></span>
<span data-ttu-id="32bb2-205">Všimněte si, že IP adresa brány hello, předponu adresy a adresu partnerského vztahu BGP pro bránu místní sítě druhý hello se nesmí překrývat s hello předchozí brána místní sítě pro hello stejné místní síti.</span><span class="sxs-lookup"><span data-stu-id="32bb2-205">Note that hello gateway IP address, address prefix, and BGP peering address for hello second local network gateway must not overlap with hello previous local network gateway for hello same on-premises network.</span></span>

```powershell
$LNGName52 = "Site5_2"
$LNGPrefix52 = "10.52.255.254/32"
$LNGIP52 = "131.107.72.23"
$BGPPeerIP52 = "10.52.255.254"

New-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
```

#### <a name="2-connect-hello-vnet-gateway-and-hello-second-local-network-gateway"></a><span data-ttu-id="32bb2-206">2. Připojení brány virtuální sítě hello a druhá brána místní sítě hello</span><span class="sxs-lookup"><span data-stu-id="32bb2-206">2. Connect hello VNet gateway and hello second local network gateway</span></span>
<span data-ttu-id="32bb2-207">Vytvoření hello připojení z virtuální sítě TestVNet1 tooSite5_2 s "EnableBGP" nastavit také$ True</span><span class="sxs-lookup"><span data-stu-id="32bb2-207">Create hello connection from TestVNet1 tooSite5_2 with "EnableBGP" set too$True</span></span>

```powershell
$lng5gw2 = Get-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-second-on-premises-vpn-device"></a><span data-ttu-id="32bb2-208">3. Připojení VPN a BGP parametry pro druhý místní zařízení VPN</span><span class="sxs-lookup"><span data-stu-id="32bb2-208">3. VPN and BGP parameters for your second on-premises VPN device</span></span>
<span data-ttu-id="32bb2-209">Níže uvedené parametry hello seznamy Podobně bude zadejte do druhé zařízení VPN hello:</span><span class="sxs-lookup"><span data-stu-id="32bb2-209">Similarly, below lists hello parameters you will enter into hello second VPN device:</span></span>

```
- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes tooannounce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP 1  : 10.12.255.4 for tunnel too40.112.190.5
- Azure VNet BGP IP 2  : 10.12.255.5 for tunnel too138.91.156.129
- Static routes        : Destination 10.12.255.4/32, nexthop hello VPN tunnel interface too40.112.190.5
                         Destination 10.12.255.5/32, nexthop hello VPN tunnel interface too138.91.156.129
- eBGP Multihop        : Ensure hello "multihop" option for eBGP is enabled on your device if needed
```

<span data-ttu-id="32bb2-210">Jakmile se navázat připojení hello (tunely), budete mít dva redundantní zařízení VPN a tunely připojení vaší místní sítí a Azure:</span><span class="sxs-lookup"><span data-stu-id="32bb2-210">Once hello connection (tunnels) are established, you will have dual redundant VPN devices and tunnels connecting your on-premises network and Azure:</span></span>

![Dual redundance crossprem](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)

## <span data-ttu-id="32bb2-212"><a name ="aav2v"></a>Část 3 – vytvoření aktivní aktivní připojení VNet-to-VNet</span><span class="sxs-lookup"><span data-stu-id="32bb2-212"><a name ="aav2v"></a>Part 3 - Establish an active-active VNet-to-VNet connection</span></span>
<span data-ttu-id="32bb2-213">V této části vytvoříme připojení k aktivní aktivní VNet-to-VNet s protokolem BGP.</span><span class="sxs-lookup"><span data-stu-id="32bb2-213">This section creates an active-active VNet-to-VNet connection with BGP.</span></span> 

<span data-ttu-id="32bb2-214">níže uvedené pokyny Hello pokračovat od hello kroků uvedených výše.</span><span class="sxs-lookup"><span data-stu-id="32bb2-214">hello instructions below continue from hello previous steps listed above.</span></span> <span data-ttu-id="32bb2-215">Je třeba provést [část 1](#aagateway) toocreate a konfigurace virtuální sítě TestVNet1 a hello brány VPN s protokolem BGP.</span><span class="sxs-lookup"><span data-stu-id="32bb2-215">You must complete [Part 1](#aagateway) toocreate and configure TestVNet1 and hello VPN Gateway with BGP.</span></span> 

### <a name="step-1---create-testvnet2-and-hello-vpn-gateway"></a><span data-ttu-id="32bb2-216">Krok 1 – Vytvoření brány VPN TestVNet2 a hello</span><span class="sxs-lookup"><span data-stu-id="32bb2-216">Step 1 - Create TestVNet2 and hello VPN gateway</span></span>
<span data-ttu-id="32bb2-217">Je důležité toomake jistotu, že hello adresní prostor IP adres hello nové virtuální sítě, TestVNet2, nepřekrývá s žádným z rozsahů vaší virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="32bb2-217">It is important toomake sure that hello IP address space of hello new virtual network, TestVNet2, does not overlap with any of your VNet ranges.</span></span>

<span data-ttu-id="32bb2-218">V tomto příkladu patří virtuální sítě hello toohello stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="32bb2-218">In this example, hello virtual networks belong toohello same subscription.</span></span> <span data-ttu-id="32bb2-219">Můžete nastavit připojení VNet-to-VNet mezi různých předplatných; naleznete příliš[konfigurace připojení typu VNet-to-VNet](vpn-gateway-vnet-vnet-rm-ps.md) toolearn více podrobností.</span><span class="sxs-lookup"><span data-stu-id="32bb2-219">You can set up VNet-to-VNet connections between different subscriptions; please refer too[Configure a VNet-to-VNet connection](vpn-gateway-vnet-vnet-rm-ps.md) toolearn more details.</span></span> <span data-ttu-id="32bb2-220">Zajistěte, aby přidáte hello "-EnableBgp $True" při vytváření hello tooenable připojení protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="32bb2-220">Make sure you add hello "-EnableBgp $True" when creating hello connections tooenable BGP.</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="32bb2-221">1. Deklarace proměnných</span><span class="sxs-lookup"><span data-stu-id="32bb2-221">1. Declare your variables</span></span>
<span data-ttu-id="32bb2-222">Být jisti tooreplace hello hodnoty hello ty, které jsou chcete toouse pro vaši konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="32bb2-222">Be sure tooreplace hello values with hello ones that you want toouse for your configuration.</span></span>

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

#### <a name="2-create-testvnet2-in-hello-new-resource-group"></a><span data-ttu-id="32bb2-223">2. Vytvoření TestVNet2 v hello novou skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="32bb2-223">2. Create TestVNet2 in hello new resource group</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2
```

#### <a name="3-create-hello-active-active-vpn-gateway-for-testvnet2"></a><span data-ttu-id="32bb2-224">3. Vytvoření brány VPN hello aktivní aktivní pro TestVNet2</span><span class="sxs-lookup"><span data-stu-id="32bb2-224">3. Create hello active-active VPN gateway for TestVNet2</span></span>
<span data-ttu-id="32bb2-225">Žádost o dvě veřejné IP adresy toobe přidělené toohello brány, které vytvoříte pro virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="32bb2-225">Request two public IP addresses toobe allocated toohello gateway you will create for your VNet.</span></span> <span data-ttu-id="32bb2-226">Budete také definovat hello podsíť a požadované konfigurace protokolu IP.</span><span class="sxs-lookup"><span data-stu-id="32bb2-226">You'll also define hello subnet and IP configurations required.</span></span>

```powershell
$gw2pip1 = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
$gw2pip2 = New-AzureRmPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2 = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
$gw2ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2
```

<span data-ttu-id="32bb2-227">Vytvoření brány VPN hello s hello jako číslo a hello příznak "EnableActiveActiveFeature".</span><span class="sxs-lookup"><span data-stu-id="32bb2-227">Create hello VPN gateway with hello AS number and hello "EnableActiveActiveFeature" flag.</span></span> <span data-ttu-id="32bb2-228">Poznámka: na Azure VPN Gateway, je nutné přepsat hello výchozí číslo ASN.</span><span class="sxs-lookup"><span data-stu-id="32bb2-228">Note that you must override hello default ASN on your Azure VPN gateways.</span></span> <span data-ttu-id="32bb2-229">Hello čísla ASN pro hello připojené virtuální sítě musí být jiný tooenable protokolu BGP a směrování přenosu.</span><span class="sxs-lookup"><span data-stu-id="32bb2-229">hello ASNs for hello connected VNets must be different tooenable BGP and transit routing.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet2ASN -EnableActiveActiveFeature
```

### <a name="step-2---connect-hello-testvnet1-and-testvnet2-gateways"></a><span data-ttu-id="32bb2-230">Krok 2 – připojení brány virtuální sítě TestVNet1 a TestVNet2 hello</span><span class="sxs-lookup"><span data-stu-id="32bb2-230">Step 2 - Connect hello TestVNet1 and TestVNet2 gateways</span></span>
<span data-ttu-id="32bb2-231">V tomto příkladu jsou obě brány v hello stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="32bb2-231">In this example, both gateways are in hello same subscription.</span></span> <span data-ttu-id="32bb2-232">Můžete použít tento krok v hello stejné relace prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="32bb2-232">You can complete this step in hello same PowerShell session.</span></span>

#### <a name="1-get-both-gateways"></a><span data-ttu-id="32bb2-233">1. Získat obě brány</span><span class="sxs-lookup"><span data-stu-id="32bb2-233">1. Get both gateways</span></span>
<span data-ttu-id="32bb2-234">Ujistěte se, přihlásit a připojte tooSubscription 1.</span><span class="sxs-lookup"><span data-stu-id="32bb2-234">Make sure you log in and connect tooSubscription 1.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
```

#### <a name="2-create-both-connections"></a><span data-ttu-id="32bb2-235">2. Vytvoření obě připojení</span><span class="sxs-lookup"><span data-stu-id="32bb2-235">2. Create both connections</span></span>
<span data-ttu-id="32bb2-236">V tomto kroku vytvoříte hello připojení z virtuální sítě TestVNet1 tooTestVNet2 a hello připojení z TestVNet2 tooTestVNet1.</span><span class="sxs-lookup"><span data-stu-id="32bb2-236">In this step, you will create hello connection from TestVNet1 tooTestVNet2, and hello connection from TestVNet2 tooTestVNet1.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> <span data-ttu-id="32bb2-237">Být jisti tooenable protokolu BGP pro obě připojení.</span><span class="sxs-lookup"><span data-stu-id="32bb2-237">Be sure tooenable BGP for BOTH connections.</span></span>
> 
> 

<span data-ttu-id="32bb2-238">Po dokončení těchto kroků, připojení hello navážou za pár minut a hello relaci partnerského vztahu protokolu BGP bude až po dokončení připojení hello VNet-to-VNet s duální redundance:</span><span class="sxs-lookup"><span data-stu-id="32bb2-238">After completing these steps, hello connection will be establish in a few minutes, and hello BGP peering session will be up once hello VNet-to-VNet connection is completed with dual redundancy:</span></span>

![aktivní aktivní v2v](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <span data-ttu-id="32bb2-240"><a name ="aaupdate"></a>Část 4 - aktualizace mezi aktivní aktivní a aktivní pohotovostní existující bránu</span><span class="sxs-lookup"><span data-stu-id="32bb2-240"><a name ="aaupdate"></a>Part 4 - Update existing gateway between active-active and active-standby</span></span>
<span data-ttu-id="32bb2-241">poslední část Hello popíše, jak můžete nakonfigurovat existující bránu Azure VPN z aktivní pohotovostní režim tooactive aktivní, nebo naopak.</span><span class="sxs-lookup"><span data-stu-id="32bb2-241">hello last section will describe how you can configure an existing Azure VPN gateway from active-standby tooactive-active mode, or vice versa.</span></span>

> [!NOTE]
> <span data-ttu-id="32bb2-242">Tato část obsahuje kroky tooresize hello starší verze skladové jednotky (SKU staré) již vytvořené brány VPN ze standardní tooHighPerformance.</span><span class="sxs-lookup"><span data-stu-id="32bb2-242">This section includes hello steps tooresize a legacy SKU (old SKU) of an already created VPN gateway from Standard tooHighPerformance.</span></span> <span data-ttu-id="32bb2-243">Tyto kroky neupgradovat původní starší verze SKU po tooone hello nové SKU.</span><span class="sxs-lookup"><span data-stu-id="32bb2-243">These steps do not upgrade an old legacy SKU tooone of hello new SKUs.</span></span>
> 
> 

### <a name="configure-an-active-standby-gateway-tooactive-active-gateway"></a><span data-ttu-id="32bb2-244">Konfigurace brány tooactive aktivní aktivní pohotovostní brány</span><span class="sxs-lookup"><span data-stu-id="32bb2-244">Configure an active-standby gateway tooactive-active gateway</span></span>
#### <a name="1-gateway-parameters"></a><span data-ttu-id="32bb2-245">1. Parametry brány</span><span class="sxs-lookup"><span data-stu-id="32bb2-245">1. Gateway parameters</span></span>
<span data-ttu-id="32bb2-246">Následující ukázka Hello převede bránu aktivní pohotovostní bránu aktivní aktivní.</span><span class="sxs-lookup"><span data-stu-id="32bb2-246">hello following example converts an active-standby gateway into an active-active gateway.</span></span> <span data-ttu-id="32bb2-247">Třeba toocreate jinou veřejnou IP adresu a pak přidejte druhý konfigurace IP brány.</span><span class="sxs-lookup"><span data-stu-id="32bb2-247">You need toocreate another public IP address, then add a second Gateway IP configuration.</span></span> <span data-ttu-id="32bb2-248">Níže znázorňuje hello používají parametry:</span><span class="sxs-lookup"><span data-stu-id="32bb2-248">Below shows hello parameters used:</span></span>

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

#### <a name="2-create-hello-public-ip-address-then-add-hello-second-gateway-ip-configuration"></a><span data-ttu-id="32bb2-249">2. Vytvoření hello veřejnou IP adresu a pak přidejte druhý konfigurace IP brány hello</span><span class="sxs-lookup"><span data-stu-id="32bb2-249">2. Create hello public IP address, then add hello second gateway IP configuration</span></span>

```powershell
$gwpip2 = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
Add-AzureRmVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2
```

#### <a name="3-enable-active-active-mode-and-update-hello-gateway"></a><span data-ttu-id="32bb2-250">3. Povolit aktivní aktivní režim a aktualizace hello brány</span><span class="sxs-lookup"><span data-stu-id="32bb2-250">3. Enable active-active mode and update hello gateway</span></span>
<span data-ttu-id="32bb2-251">Objekt hello brány musí být nastavena v prostředí PowerShell tootrigger hello skutečné aktualizace.</span><span class="sxs-lookup"><span data-stu-id="32bb2-251">You must set hello gateway object in PowerShell tootrigger hello actual update.</span></span> <span data-ttu-id="32bb2-252">Hello skladová položka brány virtuální sítě hello musí být také změněno (změněnou) tooHighPerformance vzhledem k tomu, že byla dříve vytvořena jako Standard.</span><span class="sxs-lookup"><span data-stu-id="32bb2-252">hello SKU of hello virtual network gateway must also be changed (resized) tooHighPerformance since it was created previously as Standard.</span></span>

```powershell
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature -GatewaySku HighPerformance
```

<span data-ttu-id="32bb2-253">Tato aktualizace může trvat 30 minut too45.</span><span class="sxs-lookup"><span data-stu-id="32bb2-253">This update can take 30 too45 minutes.</span></span>

### <a name="configure-an-active-active-gateway-tooactive-standby-gateway"></a><span data-ttu-id="32bb2-254">Konfigurace brány tooactive pohotovostní aktivní aktivní brány</span><span class="sxs-lookup"><span data-stu-id="32bb2-254">Configure an active-active gateway tooactive-standby gateway</span></span>
#### <a name="1-gateway-parameters"></a><span data-ttu-id="32bb2-255">1. Parametry brány</span><span class="sxs-lookup"><span data-stu-id="32bb2-255">1. Gateway parameters</span></span>
<span data-ttu-id="32bb2-256">Použití hello stejné parametry, jak je uvedeno výše, získat název hello konfigurace protokolu IP hello chcete tooremove.</span><span class="sxs-lookup"><span data-stu-id="32bb2-256">Use hello same parameters as above, get hello name of hello IP configuration you want tooremove.</span></span>

```powershell
$GWName = "TestVNetAA1GW"
$RG = "TestVPNActiveActive01"

$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
$ipconfname = $gw.IpConfigurations[1].Name
```

#### <a name="2-remove-hello-gateway-ip-configuration-and-disable-hello-active-active-mode"></a><span data-ttu-id="32bb2-257">2. Odeberte konfigurace IP brány hello a zakázat režim aktivní aktivní hello</span><span class="sxs-lookup"><span data-stu-id="32bb2-257">2. Remove hello gateway IP configuration and disable hello active-active mode</span></span>
<span data-ttu-id="32bb2-258">Podobně je nutné nastavit hello brány objekt v prostředí PowerShell tootrigger hello skutečné aktualizace.</span><span class="sxs-lookup"><span data-stu-id="32bb2-258">Similarly, you must set hello gateway object in PowerShell tootrigger hello actual update.</span></span>

```powershell
Remove-AzureRmVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature
```

<span data-ttu-id="32bb2-259">Tato aktualizace může trvat až too30 příliš 45 minut.</span><span class="sxs-lookup"><span data-stu-id="32bb2-259">This update can take up too30 too 45 minutes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="32bb2-260">Další kroky</span><span class="sxs-lookup"><span data-stu-id="32bb2-260">Next steps</span></span>
<span data-ttu-id="32bb2-261">Po dokončení připojení můžete přidat virtuální počítače tooyour virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="32bb2-261">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="32bb2-262">Kroky jsou uvedeny v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="32bb2-262">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>
