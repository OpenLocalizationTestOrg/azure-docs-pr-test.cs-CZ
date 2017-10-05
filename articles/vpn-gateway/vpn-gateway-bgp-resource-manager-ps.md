---
title: "Nakonfigurovat protokol BGP na branách Azure VPN: Správce prostředků: prostředí PowerShell | Microsoft Docs"
description: "Tento článek vás provede procesem konfigurace protokolu BGP s Azure VPN Gateway pomocí Azure Resource Manageru a prostředí PowerShell."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 905b11a7-1333-482c-820b-0fd0f44238e5
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: yushwang
ms.openlocfilehash: b00a3fe7ba4b12c2e9c486188c292cd6fafb60a3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-configure-bgp-on-azure-vpn-gateways-using-powershell"></a><span data-ttu-id="b8213-103">Postup konfigurace protokolu BGP na Azure VPN Gateway pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="b8213-103">How to configure BGP on Azure VPN Gateways using PowerShell</span></span>
<span data-ttu-id="b8213-104">Tento článek vás provede kroky k povolení protokolu BGP na připojení Site-to-Site (S2S) VPN mezi různými místy a připojení VNet-to-VNet pomocí modelu nasazení Resource Manager a prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b8213-104">This article walks you through the steps to enable BGP on a cross-premises Site-to-Site (S2S) VPN connection and a VNet-to-VNet connection using the Resource Manager deployment model and PowerShell.</span></span>

## <a name="about-bgp"></a><span data-ttu-id="b8213-105">Informace o protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="b8213-105">About BGP</span></span>
<span data-ttu-id="b8213-106">BGP je standardní směrovací protokol, na internetu běžně používaný k výměně informací o směrování a dostupnosti mezi dvěma nebo více sítěmi.</span><span class="sxs-lookup"><span data-stu-id="b8213-106">BGP is the standard routing protocol commonly used in the Internet to exchange routing and reachability information between two or more networks.</span></span> <span data-ttu-id="b8213-107">Protokol BGP umožňuje Azure VPN Gateway a vaše místní zařízení VPN, nazývají partnerské uzly protokolu BGP nebo Sousedé BGP, výměnu "tras" informujících obě brány o dostupnosti a dosažitelnosti předpon, které procházejí těmito bránami nebo trasami související se situací.</span><span class="sxs-lookup"><span data-stu-id="b8213-107">BGP enables the Azure VPN Gateways and your on-premises VPN devices, called BGP peers or neighbors, to exchange "routes" that will inform both gateways on the availability and reachability for those prefixes to go through the gateways or routers involved.</span></span> <span data-ttu-id="b8213-108">Protokol BGP také umožňuje směrování přenosu mezi více sítěmi pomocí šíření tras, které brána s protokolem BGP zjistí od jednoho partnerského uzlu protokolu BGP, do všech dalších partnerských uzlů protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="b8213-108">BGP can also enable transit routing among multiple networks by propagating routes a BGP gateway learns from one BGP peer to all other BGP peers.</span></span>

<span data-ttu-id="b8213-109">V tématu [přehled protokolu BGP službou Azure VPN Gateways](vpn-gateway-bgp-overview.md) pro další zabývat výhody protokolu BGP a pochopit technickými požadavky a důležité informace o použití protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="b8213-109">See [Overview of BGP with Azure VPN Gateways](vpn-gateway-bgp-overview.md) for more discussion on benefits of BGP and to understand the technical requirements and considerations of using BGP.</span></span>

## <a name="getting-started-with-bgp-on-azure-vpn-gateways"></a><span data-ttu-id="b8213-110">Začínáme s protokolem BGP na branách Azure VPN</span><span class="sxs-lookup"><span data-stu-id="b8213-110">Getting started with BGP on Azure VPN gateways</span></span>

<span data-ttu-id="b8213-111">Tento článek vás provede kroky, jak provést následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="b8213-111">This article walks you through the steps to do the following tasks:</span></span>

* [<span data-ttu-id="b8213-112">Část 1 - povolit protokol BGP na bráně Azure VPN</span><span class="sxs-lookup"><span data-stu-id="b8213-112">Part 1 - Enable BGP on your Azure VPN gateway</span></span>](#enablebgp)
* [<span data-ttu-id="b8213-113">Část 2 – navázání připojení mezi různými místy pomocí protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="b8213-113">Part 2 - Establish a cross-premises connection with BGP</span></span>](#crossprembgp)
* [<span data-ttu-id="b8213-114">Část 3 – připojení VNet-to-VNet s protokolem BGP</span><span class="sxs-lookup"><span data-stu-id="b8213-114">Part 3 - Establish a VNet-to-VNet connection with BGP</span></span>](#v2vbgp)

<span data-ttu-id="b8213-115">Jednotlivých součástí pokynů tvoří základní stavební blok pro povolení protokolu BGP v připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="b8213-115">Each part of the instructions forms a basic building block for enabling BGP in your network connectivity.</span></span> <span data-ttu-id="b8213-116">Pokud dokončíte všechny tři části, jak je znázorněno v následujícím diagramu sestavení topologie:</span><span class="sxs-lookup"><span data-stu-id="b8213-116">If you complete all three parts, you build the topology as shown in the following diagram:</span></span>

![Topologii BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

<span data-ttu-id="b8213-118">Můžete kombinovat částí společně k vytvoření složitějších, vícenásobného předávání přenosu síti, která vyhovuje vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="b8213-118">You can combine parts together to build a more complex, multi-hop, transit network that meets your needs.</span></span>

## <span data-ttu-id="b8213-119"><a name ="enablebgp"></a>Část 1: Konfigurace protokolu BGP na bráně Azure VPN</span><span class="sxs-lookup"><span data-stu-id="b8213-119"><a name ="enablebgp"></a>Part 1 - Configure BGP on the Azure VPN Gateway</span></span>
<span data-ttu-id="b8213-120">Postup konfigurace nastavení parametrů protokolu BGP brány Azure VPN, jak je znázorněno v následujícím diagramu:</span><span class="sxs-lookup"><span data-stu-id="b8213-120">The configuration steps set up the BGP parameters of the Azure VPN gateway as shown in the following diagram:</span></span>

![Protokol BGP brány](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a><span data-ttu-id="b8213-122">Než začnete</span><span class="sxs-lookup"><span data-stu-id="b8213-122">Before you begin</span></span>
* <span data-ttu-id="b8213-123">Ověřte, že máte předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="b8213-123">Verify that you have an Azure subscription.</span></span> <span data-ttu-id="b8213-124">Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b8213-124">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="b8213-125">Nainstalujte rutiny Powershellu pro Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b8213-125">Install the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="b8213-126">Další informace o instalaci rutin PowerShellu najdete v tématu [Instalace a konfigurace Azure PowerShellu](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b8213-126">For more information about installing the PowerShell cmdlets, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span> 

### <a name="step-1---create-and-configure-vnet1"></a><span data-ttu-id="b8213-127">Krok 1 – Vytvoření a konfigurace VNet1</span><span class="sxs-lookup"><span data-stu-id="b8213-127">Step 1 - Create and configure VNet1</span></span>
#### <a name="1-declare-your-variables"></a><span data-ttu-id="b8213-128">1. Deklarace proměnných</span><span class="sxs-lookup"><span data-stu-id="b8213-128">1. Declare your variables</span></span>
<span data-ttu-id="b8213-129">Pro toto cvičení začneme deklarací proměnných.</span><span class="sxs-lookup"><span data-stu-id="b8213-129">For this exercise, we start by declaring our variables.</span></span> <span data-ttu-id="b8213-130">V následujícím příkladu jsou proměnné deklarovány s použitím hodnot pro toto cvičení.</span><span class="sxs-lookup"><span data-stu-id="b8213-130">The following example declares the variables using the values for this exercise.</span></span> <span data-ttu-id="b8213-131">Při konfiguraci pro ostrý provoz nezapomeňte nahradit hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="b8213-131">Be sure to replace the values with your own when configuring for production.</span></span> <span data-ttu-id="b8213-132">Tyto proměnné můžete použít, pokud procházíte kroky, abyste se seznámili s tímto typem konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b8213-132">You can use these variables if you are running through the steps to become familiar with this type of configuration.</span></span> <span data-ttu-id="b8213-133">Upravte proměnné a pak je zkopírujte a vložte do konzoly prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b8213-133">Modify the variables, and then copy and paste into your PowerShell console.</span></span>

```powershell
$Sub1 = "Replace_With_Your_Subcription_Name"
$RG1 = "TestBGPRG1"
$Location1 = "East US"
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
$GWIPName1 = "VNet1GWIP"
$GWIPconfName1 = "gwipconf1"
$Connection12 = "VNet1toVNet2"
$Connection15 = "VNet1toSite5"
```

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a><span data-ttu-id="b8213-134">2. Připojení k vašemu předplatnému a vytvořte novou skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="b8213-134">2. Connect to your subscription and create a new resource group</span></span>
<span data-ttu-id="b8213-135">Pokud chcete používat rutiny Resource Manageru, ujistěte se, že jste přešli do režimu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b8213-135">To use the Resource Manager cmdlets, Make sure you switch to PowerShell mode.</span></span> <span data-ttu-id="b8213-136">Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="b8213-136">For more information, see [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="b8213-137">Otevřete konzolu prostředí PowerShell a připojte se ke svému účtu.</span><span class="sxs-lookup"><span data-stu-id="b8213-137">Open your PowerShell console and connect to your account.</span></span> <span data-ttu-id="b8213-138">Připojení vám usnadní následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="b8213-138">Use the following sample to help you connect:</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a><span data-ttu-id="b8213-139">3. Vytvoření virtuální sítě TestVNet1</span><span class="sxs-lookup"><span data-stu-id="b8213-139">3. Create TestVNet1</span></span>
<span data-ttu-id="b8213-140">Následující ukázka vytvoří virtuální síť s názvem TestVNet1 a tři podsítě, jednu s názvem GatewaySubnet, jednu s názvem FrontEnd a jednu s názvem Backend.</span><span class="sxs-lookup"><span data-stu-id="b8213-140">The following sample creates a virtual network named TestVNet1 and three subnets, one called GatewaySubnet, one called FrontEnd, and one called Backend.</span></span> <span data-ttu-id="b8213-141">Při nahrazování hodnot je důležité vždy přiřadit podsíti brány konkrétní název GatewaySubnet.</span><span class="sxs-lookup"><span data-stu-id="b8213-141">When substituting values, it's important that you always name your gateway subnet specifically GatewaySubnet.</span></span> <span data-ttu-id="b8213-142">Pokud použijete jiný název, vytvoření brány se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="b8213-142">If you name it something else, your gateway creation fails.</span></span>

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1 $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-bgp-parameters"></a><span data-ttu-id="b8213-143">Krok 2 – Vytvoření brány VPN pro virtuální síť TestVNet1 s parametry protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="b8213-143">Step 2 - Create the VPN Gateway for TestVNet1 with BGP parameters</span></span>
#### <a name="1-create-the-ip-and-subnet-configurations"></a><span data-ttu-id="b8213-144">1. Vytvoření konfigurací IP adresy a podsítě</span><span class="sxs-lookup"><span data-stu-id="b8213-144">1. Create the IP and subnet configurations</span></span>
<span data-ttu-id="b8213-145">Vyžádejte si veřejnou IP adresu, která bude přidělena bráně, kterou vytvoříte pro příslušnou virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="b8213-145">Request a public IP address to be allocated to the gateway you will create for your VNet.</span></span> <span data-ttu-id="b8213-146">Budete také definovat vyžaduje podsíť a konfigurace protokolu IP.</span><span class="sxs-lookup"><span data-stu-id="b8213-146">You'll also define the required subnet and IP configurations.</span></span>

```powershell
$gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1
```

#### <a name="2-create-the-vpn-gateway-with-the-as-number"></a><span data-ttu-id="b8213-147">2. Vytvoření brány VPN s číslo AS</span><span class="sxs-lookup"><span data-stu-id="b8213-147">2. Create the VPN gateway with the AS number</span></span>
<span data-ttu-id="b8213-148">Vytvořte bránu virtuální sítě pro virtuální síť TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="b8213-148">Create the virtual network gateway for TestVNet1.</span></span> <span data-ttu-id="b8213-149">Protokol BGP vyžaduje brány VPN založené na směrování a také parametr přidání - Asn, chcete-li nastavit ASN (čísla AS) pro virtuální síť TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="b8213-149">BGP requires a Route-Based VPN gateway, and also the addition parameter, -Asn, to set the ASN (AS Number) for TestVNet1.</span></span> <span data-ttu-id="b8213-150">Pokud parametr číslo ASN nenastavíte, bude přiřazeno číslo ASN 65515.</span><span class="sxs-lookup"><span data-stu-id="b8213-150">If you do not set the ASN parameter, ASN 65515 is assigned.</span></span> <span data-ttu-id="b8213-151">Vytvoření brány může nějakou dobu trvat (30 minut nebo déle).</span><span class="sxs-lookup"><span data-stu-id="b8213-151">Creating a gateway can take a while (30 minutes or more to complete).</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN
```

#### <a name="3-obtain-the-azure-bgp-peer-ip-address"></a><span data-ttu-id="b8213-152">3. Získat adresu IP adresa partnera BGP Azure</span><span class="sxs-lookup"><span data-stu-id="b8213-152">3. Obtain the Azure BGP Peer IP address</span></span>
<span data-ttu-id="b8213-153">Po vytvoření brány, budete muset získat adresu IP adresa partnera BGP na bráně Azure VPN.</span><span class="sxs-lookup"><span data-stu-id="b8213-153">Once the gateway is created, you need to obtain the BGP Peer IP address on the Azure VPN Gateway.</span></span> <span data-ttu-id="b8213-154">Tato adresa je potřeba ke konfiguraci Azure VPN Gateway jako partnerské zařízení protokolu BGP pro vaše místní zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="b8213-154">This address is needed to configure the Azure VPN Gateway as a BGP Peer for your on-premises VPN devices.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet1gw.BgpSettingsText
```

<span data-ttu-id="b8213-155">Poslední příkaz zobrazí odpovídající konfigurace protokolu BGP na bráně VPN Azure; například:</span><span class="sxs-lookup"><span data-stu-id="b8213-155">The last command shows the corresponding BGP configurations on the Azure VPN Gateway; for example:</span></span>

```powershell
$vnet1gw.BgpSettingsText
{
    "Asn": 65010,
    "BgpPeeringAddress": "10.12.255.30",
    "PeerWeight": 0
}
```

<span data-ttu-id="b8213-156">Po vytvoření brány, můžete tuto bránu k navázání připojení mezi různými místy nebo připojení VNet-to-VNet s protokolem BGP.</span><span class="sxs-lookup"><span data-stu-id="b8213-156">Once the gateway is created, you can use this gateway to establish cross-premises connection or VNet-to-VNet connection with BGP.</span></span> <span data-ttu-id="b8213-157">V následujících částech provede kroky k dokončení výkonu.</span><span class="sxs-lookup"><span data-stu-id="b8213-157">The following sections walk through the steps to complete the exercise.</span></span>

## <span data-ttu-id="b8213-158"><a name ="crossprembbgp"></a>Část 2 – navázání připojení mezi různými místy pomocí protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="b8213-158"><a name ="crossprembbgp"></a>Part 2 - Establish a cross-premises connection with BGP</span></span>

<span data-ttu-id="b8213-159">Navázat připojení mezi různými místy, musíte vytvořit bránu místní sítě představují vaše místní zařízení VPN a připojení pro připojení brány sítě VPN s bránu místní sítě.</span><span class="sxs-lookup"><span data-stu-id="b8213-159">To establish a cross-premises connection, you need to create a Local Network Gateway to represent your on-premises VPN device, and a Connection to connect the VPN gateway with the local network gateway.</span></span> <span data-ttu-id="b8213-160">Když jsou články, které vás provedou postupem, tento článek obsahuje další vlastnosti, které jsou zapotřebí zadat parametry konfigurace protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="b8213-160">While there are articles that walk you through these steps, this article contains the additional properties required to specify the BGP configuration parameters.</span></span>

![Protokol BGP pro více míst](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

<span data-ttu-id="b8213-162">Než budete pokračovat, ujistěte se, když jste dokončili [část 1](#enablebgp) tohoto cvičení.</span><span class="sxs-lookup"><span data-stu-id="b8213-162">Before proceeding, make sure you have completed [Part 1](#enablebgp) of this exercise.</span></span>

### <a name="step-1---create-and-configure-the-local-network-gateway"></a><span data-ttu-id="b8213-163">Krok 1 – Vytvoření a konfigurace brány místní sítě</span><span class="sxs-lookup"><span data-stu-id="b8213-163">Step 1 - Create and configure the local network gateway</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="b8213-164">1. Deklarace proměnných</span><span class="sxs-lookup"><span data-stu-id="b8213-164">1. Declare your variables</span></span>

<span data-ttu-id="b8213-165">Toto cvičení i nadále sestavení konfigurace znázorněné na obrázku.</span><span class="sxs-lookup"><span data-stu-id="b8213-165">This exercise continues to build the configuration shown in the diagram.</span></span> <span data-ttu-id="b8213-166">Nezapomeňte nahradit hodnoty těmi, které chcete použít pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="b8213-166">Be sure to replace the values with the ones that you want to use for your configuration.</span></span>

```powershell
$RG5 = "TestBGPRG5"
$Location5 = "East US 2"
$LNGName5 = "Site5"
$LNGPrefix50 = "10.52.255.254/32"
$LNGIP5 = "Your_VPN_Device_IP"
$LNGASN5 = 65050
$BGPPeerIP5 = "10.52.255.254"
```

<span data-ttu-id="b8213-167">Několik věcí, které si uvědomit o parametrech brány místní sítě:</span><span class="sxs-lookup"><span data-stu-id="b8213-167">A couple of things to note regarding the local network gateway parameters:</span></span>

* <span data-ttu-id="b8213-168">Brána místní sítě může být ve stejné nebo jiné umístění a skupině prostředků jako brána sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="b8213-168">The local network gateway can be in the same or different location and resource group as the VPN gateway.</span></span> <span data-ttu-id="b8213-169">Tento příklad ukazuje je v různých skupinách prostředků v různých umístěních.</span><span class="sxs-lookup"><span data-stu-id="b8213-169">This example shows them in different resource groups in different locations.</span></span>
* <span data-ttu-id="b8213-170">Minimální předponu, které je třeba deklarovat pro bránu místní sítě je adresa hostitele vaší IP adresy partnera BGP v zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="b8213-170">The minimum prefix you need to declare for the local network gateway is the host address of your BGP Peer IP address on your VPN device.</span></span> <span data-ttu-id="b8213-171">V takovém případě je /32 předponu "10.52.255.254/32".</span><span class="sxs-lookup"><span data-stu-id="b8213-171">In this case, it's a /32 prefix of "10.52.255.254/32".</span></span>
* <span data-ttu-id="b8213-172">Připomínáme je nutné použít různá čísla ASN protokolu BGP mezi vaší místní sítí a virtuální sítí VNet Azure.</span><span class="sxs-lookup"><span data-stu-id="b8213-172">As a reminder, you must use different BGP ASNs between your on-premises networks and Azure VNet.</span></span> <span data-ttu-id="b8213-173">Pokud se shodují, budete muset změnit číslo ASN vaší virtuální sítě, pokud vaše místní zařízení VPN už používá číslo Autonomního rovnocenných počítačů sousední ostatní směrovače BGP.</span><span class="sxs-lookup"><span data-stu-id="b8213-173">If they are the same, you need to change your VNet ASN if your on-premises VPN device already uses the ASN to peer with other BGP neighbors.</span></span>

<span data-ttu-id="b8213-174">Než budete pokračovat, zkontrolujte, že jste stále připojeni k předplatnému 1.</span><span class="sxs-lookup"><span data-stu-id="b8213-174">Before you continue, make sure you are still connected to Subscription 1.</span></span>

#### <a name="2-create-the-local-network-gateway-for-site5"></a><span data-ttu-id="b8213-175">2. Vytvoření brány místní sítě pro Site5</span><span class="sxs-lookup"><span data-stu-id="b8213-175">2. Create the local network gateway for Site5</span></span>

<span data-ttu-id="b8213-176">Je nutné vytvořit skupinu prostředků, pokud není vytvořená, před vytvořením brány místní sítě.</span><span class="sxs-lookup"><span data-stu-id="b8213-176">Be sure to create the resource group if it is not created, before you create the local network gateway.</span></span> <span data-ttu-id="b8213-177">Všimněte si dva další parametry pro bránu místní sítě: číslo Asn a BgpPeerAddress.</span><span class="sxs-lookup"><span data-stu-id="b8213-177">Notice the two additional parameters for the local network gateway: Asn and BgpPeerAddress.</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG5 -Location $Location5

New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
```

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a><span data-ttu-id="b8213-178">Krok 2 – připojení brány virtuální sítě a brány místní sítě</span><span class="sxs-lookup"><span data-stu-id="b8213-178">Step 2 - Connect the VNet gateway and local network gateway</span></span>

#### <a name="1-get-the-two-gateways"></a><span data-ttu-id="b8213-179">1. Získat dvě brány</span><span class="sxs-lookup"><span data-stu-id="b8213-179">1. Get the two gateways</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5
```

#### <a name="2-create-the-testvnet1-to-site5-connection"></a><span data-ttu-id="b8213-180">2. Vytvoření virtuální sítě TestVNet1 Site5 připojení</span><span class="sxs-lookup"><span data-stu-id="b8213-180">2. Create the TestVNet1 to Site5 connection</span></span>

<span data-ttu-id="b8213-181">V tomto kroku vytvoříte připojení z virtuální sítě TestVNet1 k Site5.</span><span class="sxs-lookup"><span data-stu-id="b8213-181">In this step, you create the connection from TestVNet1 to Site5.</span></span> <span data-ttu-id="b8213-182">Je nutné zadat "-EnableBGP $True" Povolit protokol BGP pro toto připojení.</span><span class="sxs-lookup"><span data-stu-id="b8213-182">You must specify "-EnableBGP $True" to enable BGP for this connection.</span></span> <span data-ttu-id="b8213-183">Jak už jsme probírali výše, je možné, že připojení protokolu BGP i bez protokolu BGP pro stejné Azure VPN Gateway.</span><span class="sxs-lookup"><span data-stu-id="b8213-183">As discussed earlier, it is possible to have both BGP and non-BGP connections for the same Azure VPN Gateway.</span></span> <span data-ttu-id="b8213-184">Pokud je protokol BGP povolený ve vlastnosti připojení, neumožní Azure pro toto připojení protokolu BGP, i když BGP parametry jsou již nakonfigurované na obě brány.</span><span class="sxs-lookup"><span data-stu-id="b8213-184">Unless BGP is enabled in the connection property, Azure will not enable BGP for this connection even though BGP parameters are already configured on both gateways.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

<span data-ttu-id="b8213-185">Následující příklad obsahuje parametry, které zadáte do protokolu BGP konfigurační oddíl na vaše místní zařízení VPN pro tento postup:</span><span class="sxs-lookup"><span data-stu-id="b8213-185">The following example lists the parameters you enter into the BGP configuration section on your on-premises VPN device for this exercise:</span></span>

```

- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP    : 10.12.255.30
- Static route         : Add a route for 10.12.255.30/32, with nexthop being the VPN tunnel interface on your device
- eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed
```

<span data-ttu-id="b8213-186">Připojení po několika minutách a relaci partnerského vztahu protokolu BGP spustí jednou IPsec připojení.</span><span class="sxs-lookup"><span data-stu-id="b8213-186">The connection is established after a few minutes, and the BGP peering session starts once the IPsec connection is established.</span></span>

## <span data-ttu-id="b8213-187"><a name ="v2vbgp"></a>Část 3 – připojení VNet-to-VNet s protokolem BGP</span><span class="sxs-lookup"><span data-stu-id="b8213-187"><a name ="v2vbgp"></a>Part 3 - Establish a VNet-to-VNet connection with BGP</span></span>

<span data-ttu-id="b8213-188">Tato část přidá připojení VNet-to-VNet s protokolem BGP, jak je znázorněno v následujícím diagramu:</span><span class="sxs-lookup"><span data-stu-id="b8213-188">This section adds a VNet-to-VNet connection with BGP, as shown in the following diagram:</span></span>

![Protokol BGP pro síť VNet-to-VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

<span data-ttu-id="b8213-190">Podle následujících pokynů pokračovat z předchozích kroků.</span><span class="sxs-lookup"><span data-stu-id="b8213-190">The following instructions continue from the previous steps.</span></span> <span data-ttu-id="b8213-191">Je třeba provést [části I](#enablebgp) vytvořit a nakonfigurovat virtuální síť TestVNet1 a bránu VPN s protokolem BGP.</span><span class="sxs-lookup"><span data-stu-id="b8213-191">You must complete [Part I](#enablebgp) to create and configure TestVNet1 and the VPN Gateway with BGP.</span></span> 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a><span data-ttu-id="b8213-192">Krok 1 – Vytvoření TestVNet2 a brány VPN</span><span class="sxs-lookup"><span data-stu-id="b8213-192">Step 1 - Create TestVNet2 and the VPN gateway</span></span>

<span data-ttu-id="b8213-193">Je důležité se ujistit, že adresní prostor IP adres nové virtuální sítě, TestVNet2, nepřekrývá s žádným z rozsahů vaší virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="b8213-193">It is important to make sure that the IP address space of the new virtual network, TestVNet2, does not overlap with any of your VNet ranges.</span></span>

<span data-ttu-id="b8213-194">V tomto příkladu patří virtuální sítě ve stejném předplatném.</span><span class="sxs-lookup"><span data-stu-id="b8213-194">In this example, the virtual networks belong to the same subscription.</span></span> <span data-ttu-id="b8213-195">Můžete nastavit připojení VNet-to-VNet mezi různých předplatných.</span><span class="sxs-lookup"><span data-stu-id="b8213-195">You can set up VNet-to-VNet connections between different subscriptions.</span></span> <span data-ttu-id="b8213-196">Další informace najdete v tématu [konfigurace připojení typu VNet-to-VNet](vpn-gateway-vnet-vnet-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="b8213-196">For more information, see [Configure a VNet-to-VNet connection](vpn-gateway-vnet-vnet-rm-ps.md).</span></span> <span data-ttu-id="b8213-197">Zajistěte, aby přidáte "-EnableBgp $True" při vytváření připojení k povolení protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="b8213-197">Make sure you add the "-EnableBgp $True" when creating the connections to enable BGP.</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="b8213-198">1. Deklarace proměnných</span><span class="sxs-lookup"><span data-stu-id="b8213-198">1. Declare your variables</span></span>

<span data-ttu-id="b8213-199">Nezapomeňte nahradit hodnoty těmi, které chcete použít pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="b8213-199">Be sure to replace the values with the ones that you want to use for your configuration.</span></span>

```powershell
$RG2 = "TestBGPRG2"
$Location2 = "West US"
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
$GWIPName2 = "VNet2GWIP"
$GWIPconfName2 = "gwipconf2"
$Connection21 = "VNet2toVNet1"
$Connection12 = "VNet1toVNet2"
```

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a><span data-ttu-id="b8213-200">2. Vytvoření TestVNet2 do nové skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="b8213-200">2. Create TestVNet2 in the new resource group</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2
```

#### <a name="3-create-the-vpn-gateway-for-testvnet2-with-bgp-parameters"></a><span data-ttu-id="b8213-201">3. Vytvořit bránu sítě VPN pro TestVNet2 s parametry protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="b8213-201">3. Create the VPN gateway for TestVNet2 with BGP parameters</span></span>

<span data-ttu-id="b8213-202">Požádat o veřejnou IP adresu, která bude přidělena pro bránu vytvoříte pro virtuální sítě a definujte vyžaduje podsíť a konfigurace protokolu IP.</span><span class="sxs-lookup"><span data-stu-id="b8213-202">Request a public IP address to be allocated to the gateway you will create for your VNet and define the required subnet and IP configurations.</span></span>

```powershell
$gwpip2    = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2     = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gwipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2
```

<span data-ttu-id="b8213-203">Vytvoření brány VPN s číslo AS.</span><span class="sxs-lookup"><span data-stu-id="b8213-203">Create the VPN gateway with the AS number.</span></span> <span data-ttu-id="b8213-204">Na Azure VPN Gateway je nutné přepsat výchozí číslo ASN.</span><span class="sxs-lookup"><span data-stu-id="b8213-204">You must override the default ASN on your Azure VPN gateways.</span></span> <span data-ttu-id="b8213-205">Čísla ASN pro připojené virtuální sítě se musí lišit povolit protokol BGP a směrování přenosu.</span><span class="sxs-lookup"><span data-stu-id="b8213-205">The ASNs for the connected VNets must be different to enable BGP and transit routing.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard -Asn $VNet2ASN
```

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a><span data-ttu-id="b8213-206">Krok 2 – připojení brány virtuální sítě TestVNet1 a TestVNet2</span><span class="sxs-lookup"><span data-stu-id="b8213-206">Step 2 - Connect the TestVNet1 and TestVNet2 gateways</span></span>

<span data-ttu-id="b8213-207">V tomto příkladu jsou obě brány ve stejném předplatném.</span><span class="sxs-lookup"><span data-stu-id="b8213-207">In this example, both gateways are in the same subscription.</span></span> <span data-ttu-id="b8213-208">Tento krok můžete provést ve stejné relaci prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b8213-208">You can complete this step in the same PowerShell session.</span></span>

#### <a name="1-get-both-gateways"></a><span data-ttu-id="b8213-209">1. Získat obě brány</span><span class="sxs-lookup"><span data-stu-id="b8213-209">1. Get both gateways</span></span>

<span data-ttu-id="b8213-210">Ujistěte se, že jste přihlášeni a připojeni k předplatnému 1.</span><span class="sxs-lookup"><span data-stu-id="b8213-210">Make sure you log in and connect to Subscription 1.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
```

#### <a name="2-create-both-connections"></a><span data-ttu-id="b8213-211">2. Vytvoření obě připojení</span><span class="sxs-lookup"><span data-stu-id="b8213-211">2. Create both connections</span></span>

<span data-ttu-id="b8213-212">V tomto kroku vytvoříte připojení z virtuální sítě TestVNet1 k TestVNet2 a připojení z TestVNet2 do virtuální sítě TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="b8213-212">In this step, you create the connection from TestVNet1 to TestVNet2, and the connection from TestVNet2 to TestVNet1.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> <span data-ttu-id="b8213-213">Je nutné povolit protokol BGP pro obě připojení.</span><span class="sxs-lookup"><span data-stu-id="b8213-213">Be sure to enable BGP for BOTH connections.</span></span>
> 
> 

<span data-ttu-id="b8213-214">Po dokončení těchto kroků, připojení po několika minutách.</span><span class="sxs-lookup"><span data-stu-id="b8213-214">After completing these steps, the connection is established after a few minutes.</span></span> <span data-ttu-id="b8213-215">Relaci partnerského vztahu protokolu BGP je až po dokončení připojení VNet-to-VNet.</span><span class="sxs-lookup"><span data-stu-id="b8213-215">The BGP peering session is up once the VNet-to-VNet connection is completed.</span></span>

<span data-ttu-id="b8213-216">Pokud jste dokončili všechny tři části tohoto cvičení, jste vytvořili následující topologie sítě:</span><span class="sxs-lookup"><span data-stu-id="b8213-216">If you completed all three parts of this exercise, you have established the following network topology:</span></span>

![Protokol BGP pro síť VNet-to-VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## <a name="next-steps"></a><span data-ttu-id="b8213-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b8213-218">Next steps</span></span>

<span data-ttu-id="b8213-219">Po dokončení připojení můžete do virtuálních sítí přidávat virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="b8213-219">Once your connection is complete, you can add virtual machines to your virtual networks.</span></span> <span data-ttu-id="b8213-220">Kroky jsou uvedeny v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b8213-220">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>