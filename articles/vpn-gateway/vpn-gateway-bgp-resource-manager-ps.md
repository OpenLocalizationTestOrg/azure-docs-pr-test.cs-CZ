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
ms.openlocfilehash: a9d13ae6b319e2efa8965dc2955c9b89ac3fd12b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-bgp-on-azure-vpn-gateways-using-powershell"></a><span data-ttu-id="4602f-103">Jak tooconfigure BGP na Azure VPN Gateway pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="4602f-103">How tooconfigure BGP on Azure VPN Gateways using PowerShell</span></span>
<span data-ttu-id="4602f-104">Tento článek vás provede kroky tooenable hello protokolu BGP na připojení Site-to-Site (S2S) VPN mezi různými místy a připojení VNet-to-VNet pomocí modelu nasazení Resource Manager hello a prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4602f-104">This article walks you through hello steps tooenable BGP on a cross-premises Site-to-Site (S2S) VPN connection and a VNet-to-VNet connection using hello Resource Manager deployment model and PowerShell.</span></span>

## <a name="about-bgp"></a><span data-ttu-id="4602f-105">Informace o protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="4602f-105">About BGP</span></span>
<span data-ttu-id="4602f-106">BGP je standardní směrovací protokol hello běžně používaný v hello Internet tooexchange směrování a dostupnosti informací mezi dvěma nebo více sítěmi.</span><span class="sxs-lookup"><span data-stu-id="4602f-106">BGP is hello standard routing protocol commonly used in hello Internet tooexchange routing and reachability information between two or more networks.</span></span> <span data-ttu-id="4602f-107">Protokol BGP umožňuje hello Azure VPN Gateway a vaše místní zařízení VPN, nazývají partnerské uzly protokolu BGP nebo Sousedé BGP, tooexchange "tras", bude informovat obě brány o dostupnosti hello a dostupnosti pro ty předpony toogo prostřednictvím hello bránami nebo trasami související se situací.</span><span class="sxs-lookup"><span data-stu-id="4602f-107">BGP enables hello Azure VPN Gateways and your on-premises VPN devices, called BGP peers or neighbors, tooexchange "routes" that will inform both gateways on hello availability and reachability for those prefixes toogo through hello gateways or routers involved.</span></span> <span data-ttu-id="4602f-108">Protokol BGP můžete také povolit směrování přenosu mezi více sítěmi pomocí šíření tras, které brána s protokolem BGP zjistí od jednoho tooall partnera BGP ostatním partnerům BGP.</span><span class="sxs-lookup"><span data-stu-id="4602f-108">BGP can also enable transit routing among multiple networks by propagating routes a BGP gateway learns from one BGP peer tooall other BGP peers.</span></span>

<span data-ttu-id="4602f-109">V tématu [přehled protokolu BGP službou Azure VPN Gateways](vpn-gateway-bgp-overview.md) pro další zabývat výhody protokol BGP a toounderstand hello technické požadavky a důležité informace o použití protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="4602f-109">See [Overview of BGP with Azure VPN Gateways](vpn-gateway-bgp-overview.md) for more discussion on benefits of BGP and toounderstand hello technical requirements and considerations of using BGP.</span></span>

## <a name="getting-started-with-bgp-on-azure-vpn-gateways"></a><span data-ttu-id="4602f-110">Začínáme s protokolem BGP na branách Azure VPN</span><span class="sxs-lookup"><span data-stu-id="4602f-110">Getting started with BGP on Azure VPN gateways</span></span>

<span data-ttu-id="4602f-111">Tento článek vás provede hello kroky toodo hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="4602f-111">This article walks you through hello steps toodo hello following tasks:</span></span>

* [<span data-ttu-id="4602f-112">Část 1 - povolit protokol BGP na bráně Azure VPN</span><span class="sxs-lookup"><span data-stu-id="4602f-112">Part 1 - Enable BGP on your Azure VPN gateway</span></span>](#enablebgp)
* [<span data-ttu-id="4602f-113">Část 2 – navázání připojení mezi různými místy pomocí protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="4602f-113">Part 2 - Establish a cross-premises connection with BGP</span></span>](#crossprembgp)
* [<span data-ttu-id="4602f-114">Část 3 – připojení VNet-to-VNet s protokolem BGP</span><span class="sxs-lookup"><span data-stu-id="4602f-114">Part 3 - Establish a VNet-to-VNet connection with BGP</span></span>](#v2vbgp)

<span data-ttu-id="4602f-115">Jednotlivých součástí hello pokyny tvoří základní stavební blok pro povolení protokolu BGP v připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="4602f-115">Each part of hello instructions forms a basic building block for enabling BGP in your network connectivity.</span></span> <span data-ttu-id="4602f-116">Pokud dokončíte všechny tři části, jak je znázorněno v následujícím diagramu hello sestavení hello topologie:</span><span class="sxs-lookup"><span data-stu-id="4602f-116">If you complete all three parts, you build hello topology as shown in hello following diagram:</span></span>

![Topologii BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

<span data-ttu-id="4602f-118">Můžete kombinovat částí společně toobuild složitější, vícenásobného předávání tranzitní sítě, která vyhovuje vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="4602f-118">You can combine parts together toobuild a more complex, multi-hop, transit network that meets your needs.</span></span>

## <span data-ttu-id="4602f-119"><a name ="enablebgp"></a>Část 1: Konfigurace protokolu BGP na hello Azure VPN Gateway</span><span class="sxs-lookup"><span data-stu-id="4602f-119"><a name ="enablebgp"></a>Part 1 - Configure BGP on hello Azure VPN Gateway</span></span>
<span data-ttu-id="4602f-120">Postup konfigurace Hello nastavit hello parametry protokolu BGP brány Azure VPN hello jak je znázorněno v následujícím diagramu hello:</span><span class="sxs-lookup"><span data-stu-id="4602f-120">hello configuration steps set up hello BGP parameters of hello Azure VPN gateway as shown in hello following diagram:</span></span>

![Protokol BGP brány](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a><span data-ttu-id="4602f-122">Než začnete</span><span class="sxs-lookup"><span data-stu-id="4602f-122">Before you begin</span></span>
* <span data-ttu-id="4602f-123">Ověřte, že máte předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="4602f-123">Verify that you have an Azure subscription.</span></span> <span data-ttu-id="4602f-124">Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4602f-124">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="4602f-125">Nainstalujte rutiny Powershellu pro Azure Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="4602f-125">Install hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="4602f-126">Další informace o instalaci rutin prostředí PowerShell hello najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4602f-126">For more information about installing hello PowerShell cmdlets, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> 

### <a name="step-1---create-and-configure-vnet1"></a><span data-ttu-id="4602f-127">Krok 1 – Vytvoření a konfigurace VNet1</span><span class="sxs-lookup"><span data-stu-id="4602f-127">Step 1 - Create and configure VNet1</span></span>
#### <a name="1-declare-your-variables"></a><span data-ttu-id="4602f-128">1. Deklarace proměnných</span><span class="sxs-lookup"><span data-stu-id="4602f-128">1. Declare your variables</span></span>
<span data-ttu-id="4602f-129">Pro toto cvičení začneme deklarací proměnných.</span><span class="sxs-lookup"><span data-stu-id="4602f-129">For this exercise, we start by declaring our variables.</span></span> <span data-ttu-id="4602f-130">Hello následující příklad deklaruje hello proměnné pomocí hello hodnot pro toto cvičení.</span><span class="sxs-lookup"><span data-stu-id="4602f-130">hello following example declares hello variables using hello values for this exercise.</span></span> <span data-ttu-id="4602f-131">Zda tooreplace hello hodnoty vlastními být při konfiguraci pro produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="4602f-131">Be sure tooreplace hello values with your own when configuring for production.</span></span> <span data-ttu-id="4602f-132">Tyto proměnné můžete použít, pokud používáte prostřednictvím toobecome kroky hello seznámili s tímto typem konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4602f-132">You can use these variables if you are running through hello steps toobecome familiar with this type of configuration.</span></span> <span data-ttu-id="4602f-133">Umožňuje změnit proměnné hello a pak zkopírujte a vložte do konzoly prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4602f-133">Modify hello variables, and then copy and paste into your PowerShell console.</span></span>

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

#### <a name="2-connect-tooyour-subscription-and-create-a-new-resource-group"></a><span data-ttu-id="4602f-134">2. Připojení tooyour odběru a vytvořit novou skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="4602f-134">2. Connect tooyour subscription and create a new resource group</span></span>
<span data-ttu-id="4602f-135">toouse hello rutiny Resource Manager, ujistěte se, že jste přešli tooPowerShell režimu.</span><span class="sxs-lookup"><span data-stu-id="4602f-135">toouse hello Resource Manager cmdlets, Make sure you switch tooPowerShell mode.</span></span> <span data-ttu-id="4602f-136">Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="4602f-136">For more information, see [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="4602f-137">Otevřete konzolu prostředí PowerShell a připojte tooyour účtu.</span><span class="sxs-lookup"><span data-stu-id="4602f-137">Open your PowerShell console and connect tooyour account.</span></span> <span data-ttu-id="4602f-138">Použijte následující ukázka toohelp, ke kterým se připojujete hello:</span><span class="sxs-lookup"><span data-stu-id="4602f-138">Use hello following sample toohelp you connect:</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a><span data-ttu-id="4602f-139">3. Vytvoření virtuální sítě TestVNet1</span><span class="sxs-lookup"><span data-stu-id="4602f-139">3. Create TestVNet1</span></span>
<span data-ttu-id="4602f-140">Hello následující ukázka vytvoří virtuální síť s názvem TestVNet1 a tři podsítě, jednu s názvem GatewaySubnet, jednu s názvem FrontEnd a jednu s názvem Backend.</span><span class="sxs-lookup"><span data-stu-id="4602f-140">hello following sample creates a virtual network named TestVNet1 and three subnets, one called GatewaySubnet, one called FrontEnd, and one called Backend.</span></span> <span data-ttu-id="4602f-141">Při nahrazování hodnot je důležité vždy přiřadit podsíti brány konkrétní název GatewaySubnet.</span><span class="sxs-lookup"><span data-stu-id="4602f-141">When substituting values, it's important that you always name your gateway subnet specifically GatewaySubnet.</span></span> <span data-ttu-id="4602f-142">Pokud použijete jiný název, vytvoření brány se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="4602f-142">If you name it something else, your gateway creation fails.</span></span>

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1 $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-hello-vpn-gateway-for-testvnet1-with-bgp-parameters"></a><span data-ttu-id="4602f-143">Krok 2 – Vytvoření hello brána sítě VPN pro virtuální síť TestVNet1 s parametry protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="4602f-143">Step 2 - Create hello VPN Gateway for TestVNet1 with BGP parameters</span></span>
#### <a name="1-create-hello-ip-and-subnet-configurations"></a><span data-ttu-id="4602f-144">1. Vytvoření hello konfigurací IP adresy a podsítě</span><span class="sxs-lookup"><span data-stu-id="4602f-144">1. Create hello IP and subnet configurations</span></span>
<span data-ttu-id="4602f-145">Požádat o veřejné IP adresy toobe přidělené toohello bránu, které vytvoříte pro virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="4602f-145">Request a public IP address toobe allocated toohello gateway you will create for your VNet.</span></span> <span data-ttu-id="4602f-146">Budete také definovat hello vyžaduje podsíť a konfigurace protokolu IP.</span><span class="sxs-lookup"><span data-stu-id="4602f-146">You'll also define hello required subnet and IP configurations.</span></span>

```powershell
$gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1
```

#### <a name="2-create-hello-vpn-gateway-with-hello-as-number"></a><span data-ttu-id="4602f-147">2. Vytvoření brány VPN hello s hello jako číslo</span><span class="sxs-lookup"><span data-stu-id="4602f-147">2. Create hello VPN gateway with hello AS number</span></span>
<span data-ttu-id="4602f-148">Vytvoření brány virtuální sítě hello pro virtuální síť TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="4602f-148">Create hello virtual network gateway for TestVNet1.</span></span> <span data-ttu-id="4602f-149">Protokol BGP vyžaduje Trasové brány sítě VPN a také hello přidání parametru - číslo Asn, tooset hello ASN (čísla AS) pro virtuální síť TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="4602f-149">BGP requires a Route-Based VPN gateway, and also hello addition parameter, -Asn, tooset hello ASN (AS Number) for TestVNet1.</span></span> <span data-ttu-id="4602f-150">Pokud parametr číslo ASN hello nenastavíte, bude přiřazeno číslo ASN 65515.</span><span class="sxs-lookup"><span data-stu-id="4602f-150">If you do not set hello ASN parameter, ASN 65515 is assigned.</span></span> <span data-ttu-id="4602f-151">Vytvoření brány může chvíli trvat (30 minut nebo další toocomplete).</span><span class="sxs-lookup"><span data-stu-id="4602f-151">Creating a gateway can take a while (30 minutes or more toocomplete).</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN
```

#### <a name="3-obtain-hello-azure-bgp-peer-ip-address"></a><span data-ttu-id="4602f-152">3. Získat adresu IP adresa partnera BGP Azure hello</span><span class="sxs-lookup"><span data-stu-id="4602f-152">3. Obtain hello Azure BGP Peer IP address</span></span>
<span data-ttu-id="4602f-153">Po vytvoření brány hello musíte tooobtain hello IP adresy partnera BGP na hello Azure VPN Gateway.</span><span class="sxs-lookup"><span data-stu-id="4602f-153">Once hello gateway is created, you need tooobtain hello BGP Peer IP address on hello Azure VPN Gateway.</span></span> <span data-ttu-id="4602f-154">Tato adresa je potřebné tooconfigure hello Azure VPN Gateway jako partnerské zařízení protokolu BGP pro vaše místní zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="4602f-154">This address is needed tooconfigure hello Azure VPN Gateway as a BGP Peer for your on-premises VPN devices.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet1gw.BgpSettingsText
```

<span data-ttu-id="4602f-155">poslední příkaz Hello zobrazí odpovídající konfigurace protokolu BGP hello na hello Azure VPN Gateway; například:</span><span class="sxs-lookup"><span data-stu-id="4602f-155">hello last command shows hello corresponding BGP configurations on hello Azure VPN Gateway; for example:</span></span>

```powershell
$vnet1gw.BgpSettingsText
{
    "Asn": 65010,
    "BgpPeeringAddress": "10.12.255.30",
    "PeerWeight": 0
}
```

<span data-ttu-id="4602f-156">Po vytvoření brány hello lze použít toto připojení mezi různými místy tooestablish brány nebo připojení VNet-to-VNet s protokolem BGP.</span><span class="sxs-lookup"><span data-stu-id="4602f-156">Once hello gateway is created, you can use this gateway tooestablish cross-premises connection or VNet-to-VNet connection with BGP.</span></span> <span data-ttu-id="4602f-157">Hello následující části provede hello kroky toocomplete hello cvičení.</span><span class="sxs-lookup"><span data-stu-id="4602f-157">hello following sections walk through hello steps toocomplete hello exercise.</span></span>

## <span data-ttu-id="4602f-158"><a name ="crossprembbgp"></a>Část 2 – navázání připojení mezi různými místy pomocí protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="4602f-158"><a name ="crossprembbgp"></a>Part 2 - Establish a cross-premises connection with BGP</span></span>

<span data-ttu-id="4602f-159">tooestablish připojení mezi různými místy, budete potřebovat toocreate toorepresent bránu místní sítě vašeho místního zařízení VPN a brány VPN připojení tooconnect hello s hello brány místní sítě.</span><span class="sxs-lookup"><span data-stu-id="4602f-159">tooestablish a cross-premises connection, you need toocreate a Local Network Gateway toorepresent your on-premises VPN device, and a Connection tooconnect hello VPN gateway with hello local network gateway.</span></span> <span data-ttu-id="4602f-160">Když jsou články, které vás provedou postupem, tento článek obsahuje parametry konfigurace protokolu BGP požadované toospecify hello aplikace hello další vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="4602f-160">While there are articles that walk you through these steps, this article contains hello additional properties required toospecify hello BGP configuration parameters.</span></span>

![Protokol BGP pro více míst](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

<span data-ttu-id="4602f-162">Než budete pokračovat, ujistěte se, když jste dokončili [část 1](#enablebgp) tohoto cvičení.</span><span class="sxs-lookup"><span data-stu-id="4602f-162">Before proceeding, make sure you have completed [Part 1](#enablebgp) of this exercise.</span></span>

### <a name="step-1---create-and-configure-hello-local-network-gateway"></a><span data-ttu-id="4602f-163">Krok 1 – Vytvoření a konfigurace brány místní sítě hello</span><span class="sxs-lookup"><span data-stu-id="4602f-163">Step 1 - Create and configure hello local network gateway</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="4602f-164">1. Deklarace proměnných</span><span class="sxs-lookup"><span data-stu-id="4602f-164">1. Declare your variables</span></span>

<span data-ttu-id="4602f-165">Toto cvičení pokračuje toobuild hello konfigurace znázorněné hello diagram.</span><span class="sxs-lookup"><span data-stu-id="4602f-165">This exercise continues toobuild hello configuration shown in hello diagram.</span></span> <span data-ttu-id="4602f-166">Být jisti tooreplace hello hodnoty hello ty, které jsou chcete toouse pro vaši konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="4602f-166">Be sure tooreplace hello values with hello ones that you want toouse for your configuration.</span></span>

```powershell
$RG5 = "TestBGPRG5"
$Location5 = "East US 2"
$LNGName5 = "Site5"
$LNGPrefix50 = "10.52.255.254/32"
$LNGIP5 = "Your_VPN_Device_IP"
$LNGASN5 = 65050
$BGPPeerIP5 = "10.52.255.254"
```

<span data-ttu-id="4602f-167">Pár věcí toonote týkající se parametrů brány místní sítě hello:</span><span class="sxs-lookup"><span data-stu-id="4602f-167">A couple of things toonote regarding hello local network gateway parameters:</span></span>

* <span data-ttu-id="4602f-168">Brána místní sítě Hello může být v hello stejné nebo jiné umístění a prostředků skupiny jako hello brány VPN.</span><span class="sxs-lookup"><span data-stu-id="4602f-168">hello local network gateway can be in hello same or different location and resource group as hello VPN gateway.</span></span> <span data-ttu-id="4602f-169">Tento příklad ukazuje je v různých skupinách prostředků v různých umístěních.</span><span class="sxs-lookup"><span data-stu-id="4602f-169">This example shows them in different resource groups in different locations.</span></span>
* <span data-ttu-id="4602f-170">minimální předponu Hello potřebujete toodeclare pro bránu místní sítě hello je adresa hostitele hello vaší IP adresy partnera BGP v zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="4602f-170">hello minimum prefix you need toodeclare for hello local network gateway is hello host address of your BGP Peer IP address on your VPN device.</span></span> <span data-ttu-id="4602f-171">V takovém případě je /32 předponu "10.52.255.254/32".</span><span class="sxs-lookup"><span data-stu-id="4602f-171">In this case, it's a /32 prefix of "10.52.255.254/32".</span></span>
* <span data-ttu-id="4602f-172">Připomínáme je nutné použít různá čísla ASN protokolu BGP mezi vaší místní sítí a virtuální sítí VNet Azure.</span><span class="sxs-lookup"><span data-stu-id="4602f-172">As a reminder, you must use different BGP ASNs between your on-premises networks and Azure VNet.</span></span> <span data-ttu-id="4602f-173">Pokud se jsou hello stejné, je třeba toochange vaší virtuální sítě ASN Pokud vaše místní zařízení VPN už používá hello ASN toopeer sousední ostatní směrovače BGP.</span><span class="sxs-lookup"><span data-stu-id="4602f-173">If they are hello same, you need toochange your VNet ASN if your on-premises VPN device already uses hello ASN toopeer with other BGP neighbors.</span></span>

<span data-ttu-id="4602f-174">Než budete pokračovat, ujistěte se, že jsou stále připojená tooSubscription 1.</span><span class="sxs-lookup"><span data-stu-id="4602f-174">Before you continue, make sure you are still connected tooSubscription 1.</span></span>

#### <a name="2-create-hello-local-network-gateway-for-site5"></a><span data-ttu-id="4602f-175">2. Vytvoření brány místní sítě hello pro Site5</span><span class="sxs-lookup"><span data-stu-id="4602f-175">2. Create hello local network gateway for Site5</span></span>

<span data-ttu-id="4602f-176">Být zda skupiny prostředků toocreate hello, pokud není vytvořená, před vytvořením brány místní sítě hello.</span><span class="sxs-lookup"><span data-stu-id="4602f-176">Be sure toocreate hello resource group if it is not created, before you create hello local network gateway.</span></span> <span data-ttu-id="4602f-177">Všimněte si hello dva další parametry pro bránu místní sítě hello: číslo Asn a BgpPeerAddress.</span><span class="sxs-lookup"><span data-stu-id="4602f-177">Notice hello two additional parameters for hello local network gateway: Asn and BgpPeerAddress.</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG5 -Location $Location5

New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
```

### <a name="step-2---connect-hello-vnet-gateway-and-local-network-gateway"></a><span data-ttu-id="4602f-178">Krok 2 – připojit hello brány virtuální sítě a brány místní sítě</span><span class="sxs-lookup"><span data-stu-id="4602f-178">Step 2 - Connect hello VNet gateway and local network gateway</span></span>

#### <a name="1-get-hello-two-gateways"></a><span data-ttu-id="4602f-179">1. Získat hello dvě brány</span><span class="sxs-lookup"><span data-stu-id="4602f-179">1. Get hello two gateways</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5
```

#### <a name="2-create-hello-testvnet1-toosite5-connection"></a><span data-ttu-id="4602f-180">2. Vytvoření připojení tooSite5 hello virtuální sítě TestVNet1</span><span class="sxs-lookup"><span data-stu-id="4602f-180">2. Create hello TestVNet1 tooSite5 connection</span></span>

<span data-ttu-id="4602f-181">V tomto kroku vytvoříte hello připojení z virtuální sítě TestVNet1 tooSite5.</span><span class="sxs-lookup"><span data-stu-id="4602f-181">In this step, you create hello connection from TestVNet1 tooSite5.</span></span> <span data-ttu-id="4602f-182">Je nutné zadat "-EnableBGP $True" tooenable protokolu BGP pro toto připojení.</span><span class="sxs-lookup"><span data-stu-id="4602f-182">You must specify "-EnableBGP $True" tooenable BGP for this connection.</span></span> <span data-ttu-id="4602f-183">Jak už jsme probírali výše, je možné toohave připojení protokolu BGP i bez protokolu BGP pro hello stejné Azure VPN Gateway.</span><span class="sxs-lookup"><span data-stu-id="4602f-183">As discussed earlier, it is possible toohave both BGP and non-BGP connections for hello same Azure VPN Gateway.</span></span> <span data-ttu-id="4602f-184">Pokud je protokol BGP povolený ve vlastnosti připojení hello, neumožní Azure pro toto připojení protokolu BGP, i když BGP parametry jsou již nakonfigurované na obě brány.</span><span class="sxs-lookup"><span data-stu-id="4602f-184">Unless BGP is enabled in hello connection property, Azure will not enable BGP for this connection even though BGP parameters are already configured on both gateways.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

<span data-ttu-id="4602f-185">Hello následující příklad obsahuje hello parametry, které zadáte do hello BGP konfigurační oddíl na vaše místní zařízení VPN pro tento postup:</span><span class="sxs-lookup"><span data-stu-id="4602f-185">hello following example lists hello parameters you enter into hello BGP configuration section on your on-premises VPN device for this exercise:</span></span>

```

- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes tooannounce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP    : 10.12.255.30
- Static route         : Add a route for 10.12.255.30/32, with nexthop being hello VPN tunnel interface on your device
- eBGP Multihop        : Ensure hello "multihop" option for eBGP is enabled on your device if needed
```

<span data-ttu-id="4602f-186">Hello připojení po několik minut a hello BGP partnerského vztahu relace spustí po vytvoření hello připojení protokolu IPsec.</span><span class="sxs-lookup"><span data-stu-id="4602f-186">hello connection is established after a few minutes, and hello BGP peering session starts once hello IPsec connection is established.</span></span>

## <span data-ttu-id="4602f-187"><a name ="v2vbgp"></a>Část 3 – připojení VNet-to-VNet s protokolem BGP</span><span class="sxs-lookup"><span data-stu-id="4602f-187"><a name ="v2vbgp"></a>Part 3 - Establish a VNet-to-VNet connection with BGP</span></span>

<span data-ttu-id="4602f-188">Tato část přidá připojení VNet-to-VNet s protokolem BGP, jak je znázorněno v následujícím diagramu hello:</span><span class="sxs-lookup"><span data-stu-id="4602f-188">This section adds a VNet-to-VNet connection with BGP, as shown in hello following diagram:</span></span>

![Protokol BGP pro síť VNet-to-VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

<span data-ttu-id="4602f-190">Hello pokynů pokračovat z předchozích kroků hello.</span><span class="sxs-lookup"><span data-stu-id="4602f-190">hello following instructions continue from hello previous steps.</span></span> <span data-ttu-id="4602f-191">Je třeba provést [části I](#enablebgp) toocreate a konfigurace virtuální sítě TestVNet1 a hello brány VPN s protokolem BGP.</span><span class="sxs-lookup"><span data-stu-id="4602f-191">You must complete [Part I](#enablebgp) toocreate and configure TestVNet1 and hello VPN Gateway with BGP.</span></span> 

### <a name="step-1---create-testvnet2-and-hello-vpn-gateway"></a><span data-ttu-id="4602f-192">Krok 1 – Vytvoření brány VPN TestVNet2 a hello</span><span class="sxs-lookup"><span data-stu-id="4602f-192">Step 1 - Create TestVNet2 and hello VPN gateway</span></span>

<span data-ttu-id="4602f-193">Je důležité toomake jistotu, že hello adresní prostor IP adres hello nové virtuální sítě, TestVNet2, nepřekrývá s žádným z rozsahů vaší virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="4602f-193">It is important toomake sure that hello IP address space of hello new virtual network, TestVNet2, does not overlap with any of your VNet ranges.</span></span>

<span data-ttu-id="4602f-194">V tomto příkladu patří virtuální sítě hello toohello stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="4602f-194">In this example, hello virtual networks belong toohello same subscription.</span></span> <span data-ttu-id="4602f-195">Můžete nastavit připojení VNet-to-VNet mezi různých předplatných.</span><span class="sxs-lookup"><span data-stu-id="4602f-195">You can set up VNet-to-VNet connections between different subscriptions.</span></span> <span data-ttu-id="4602f-196">Další informace najdete v tématu [konfigurace připojení typu VNet-to-VNet](vpn-gateway-vnet-vnet-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="4602f-196">For more information, see [Configure a VNet-to-VNet connection](vpn-gateway-vnet-vnet-rm-ps.md).</span></span> <span data-ttu-id="4602f-197">Zajistěte, aby přidáte hello "-EnableBgp $True" při vytváření hello tooenable připojení protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="4602f-197">Make sure you add hello "-EnableBgp $True" when creating hello connections tooenable BGP.</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="4602f-198">1. Deklarace proměnných</span><span class="sxs-lookup"><span data-stu-id="4602f-198">1. Declare your variables</span></span>

<span data-ttu-id="4602f-199">Být jisti tooreplace hello hodnoty hello ty, které jsou chcete toouse pro vaši konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="4602f-199">Be sure tooreplace hello values with hello ones that you want toouse for your configuration.</span></span>

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

#### <a name="2-create-testvnet2-in-hello-new-resource-group"></a><span data-ttu-id="4602f-200">2. Vytvoření TestVNet2 v hello novou skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="4602f-200">2. Create TestVNet2 in hello new resource group</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2
```

#### <a name="3-create-hello-vpn-gateway-for-testvnet2-with-bgp-parameters"></a><span data-ttu-id="4602f-201">3. Vytvoření brány VPN hello pro TestVNet2 s parametry protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="4602f-201">3. Create hello VPN gateway for TestVNet2 with BGP parameters</span></span>

<span data-ttu-id="4602f-202">Požádat o veřejné IP adresy toobe přidělené toohello bránu vytvoříte pro virtuální sítě a definujte hello vyžaduje podsíť a konfigurace protokolu IP.</span><span class="sxs-lookup"><span data-stu-id="4602f-202">Request a public IP address toobe allocated toohello gateway you will create for your VNet and define hello required subnet and IP configurations.</span></span>

```powershell
$gwpip2    = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2     = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gwipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2
```

<span data-ttu-id="4602f-203">Vytvoření brány VPN hello s hello jako číslo.</span><span class="sxs-lookup"><span data-stu-id="4602f-203">Create hello VPN gateway with hello AS number.</span></span> <span data-ttu-id="4602f-204">Na Azure VPN Gateway je nutné přepsat hello výchozí číslo ASN.</span><span class="sxs-lookup"><span data-stu-id="4602f-204">You must override hello default ASN on your Azure VPN gateways.</span></span> <span data-ttu-id="4602f-205">Hello čísla ASN pro hello připojené virtuální sítě musí být jiný tooenable protokolu BGP a směrování přenosu.</span><span class="sxs-lookup"><span data-stu-id="4602f-205">hello ASNs for hello connected VNets must be different tooenable BGP and transit routing.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard -Asn $VNet2ASN
```

### <a name="step-2---connect-hello-testvnet1-and-testvnet2-gateways"></a><span data-ttu-id="4602f-206">Krok 2 – připojení brány virtuální sítě TestVNet1 a TestVNet2 hello</span><span class="sxs-lookup"><span data-stu-id="4602f-206">Step 2 - Connect hello TestVNet1 and TestVNet2 gateways</span></span>

<span data-ttu-id="4602f-207">V tomto příkladu jsou obě brány v hello stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="4602f-207">In this example, both gateways are in hello same subscription.</span></span> <span data-ttu-id="4602f-208">Můžete použít tento krok v hello stejné relace prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4602f-208">You can complete this step in hello same PowerShell session.</span></span>

#### <a name="1-get-both-gateways"></a><span data-ttu-id="4602f-209">1. Získat obě brány</span><span class="sxs-lookup"><span data-stu-id="4602f-209">1. Get both gateways</span></span>

<span data-ttu-id="4602f-210">Ujistěte se, přihlásit a připojte tooSubscription 1.</span><span class="sxs-lookup"><span data-stu-id="4602f-210">Make sure you log in and connect tooSubscription 1.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
```

#### <a name="2-create-both-connections"></a><span data-ttu-id="4602f-211">2. Vytvoření obě připojení</span><span class="sxs-lookup"><span data-stu-id="4602f-211">2. Create both connections</span></span>

<span data-ttu-id="4602f-212">V tomto kroku vytvoříte z TestVNet2 tooTestVNet1 hello připojení z virtuální sítě TestVNet1 tooTestVNet2 a hello připojení.</span><span class="sxs-lookup"><span data-stu-id="4602f-212">In this step, you create hello connection from TestVNet1 tooTestVNet2, and hello connection from TestVNet2 tooTestVNet1.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> <span data-ttu-id="4602f-213">Být jisti tooenable protokolu BGP pro obě připojení.</span><span class="sxs-lookup"><span data-stu-id="4602f-213">Be sure tooenable BGP for BOTH connections.</span></span>
> 
> 

<span data-ttu-id="4602f-214">Po dokončení těchto kroků, hello připojení po několika minutách.</span><span class="sxs-lookup"><span data-stu-id="4602f-214">After completing these steps, hello connection is established after a few minutes.</span></span> <span data-ttu-id="4602f-215">relaci partnerského vztahu protokolu BGP Hello je až po dokončení hello připojení VNet-to-VNet.</span><span class="sxs-lookup"><span data-stu-id="4602f-215">hello BGP peering session is up once hello VNet-to-VNet connection is completed.</span></span>

<span data-ttu-id="4602f-216">Pokud jste dokončili všechny tři části tohoto cvičení, jste vytvořili hello následující topologie sítě:</span><span class="sxs-lookup"><span data-stu-id="4602f-216">If you completed all three parts of this exercise, you have established hello following network topology:</span></span>

![Protokol BGP pro síť VNet-to-VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## <a name="next-steps"></a><span data-ttu-id="4602f-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4602f-218">Next steps</span></span>

<span data-ttu-id="4602f-219">Po dokončení připojení můžete přidat virtuální počítače tooyour virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="4602f-219">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="4602f-220">Kroky jsou uvedeny v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4602f-220">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>
