---
title: "aaaAbout 3. stran VPN zařízení konfigurace tooconnect tooAzure VPN Gateway | Microsoft Docs"
description: "Tento článek obsahuje přehled konfigurace zařízení VPN 3. stran pro připojení brány VPN tooAzure."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: 
ms.assetid: a8bfc955-de49-4172-95ac-5257e262d7ea
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: yushwang
ms.openlocfilehash: 3bb4fc94bc625386c2d0a02e1dcbdeb38ee0665e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-3rd-party-vpn-device-configurations"></a><span data-ttu-id="b91dc-103">Přehled o 3. konfigurace zařízení VPN strany</span><span class="sxs-lookup"><span data-stu-id="b91dc-103">Overview of 3rd party VPN device configurations</span></span>
<span data-ttu-id="b91dc-104">Tento článek obsahuje přehled místní konfigurace zařízení VPN pro připojení brány VPN tooAzure.</span><span class="sxs-lookup"><span data-stu-id="b91dc-104">This article provides an overview of on-premises VPN device configurations for connecting tooAzure VPN gateways.</span></span> <span data-ttu-id="b91dc-105">Ukázka Hello virtuální síť Azure a bude instalační program brány sítě VPN se stejnými parametry použité tooconnect toodifferent místní zařízení VPN s hello.</span><span class="sxs-lookup"><span data-stu-id="b91dc-105">hello sample Azure virtual network and VPN gateway setup will be used tooconnect toodifferent on-premises VPN devices with hello same parameters.</span></span>

## <a name="device-requirements"></a><span data-ttu-id="b91dc-106">Požadavky na zařízení</span><span class="sxs-lookup"><span data-stu-id="b91dc-106">Device requirements</span></span>
<span data-ttu-id="b91dc-107">Azure VPN Gateway pomocí standardní sady protokolu IPsec/IKE pro tunelů S2S VPN.</span><span class="sxs-lookup"><span data-stu-id="b91dc-107">Azure VPN gateways use standard IPsec/IKE protocol suites for S2S VPN tunnels.</span></span> <span data-ttu-id="b91dc-108">Odkazovat příliš[informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md) hello podrobné parametry protokolu IPsec/IKE a výchozí kryptografické algoritmy pro Azure VPN Gateway.</span><span class="sxs-lookup"><span data-stu-id="b91dc-108">Refer too[About VPN devices](vpn-gateway-about-vpn-devices.md) for hello detailed IPsec/IKE protocol parameters and default cryptographic algorithms for Azure VPN gateways.</span></span> <span data-ttu-id="b91dc-109">Volitelně můžete zadat přesný kombinace hello kryptografické algoritmy a klíče síly pro konkrétní připojení jak je popsáno v [o požadavcích na kryptografických](vpn-gateway-about-compliance-crypto.md).</span><span class="sxs-lookup"><span data-stu-id="b91dc-109">You can optionally specify hello exact combination of cryptographic algorithms and key strengths for a specific connection as described in [About cryptographic requirements](vpn-gateway-about-compliance-crypto.md).</span></span>

## <span data-ttu-id="b91dc-110"><a name ="singletunnel"></a>Jedno tunelové propojení sítě VPN</span><span class="sxs-lookup"><span data-stu-id="b91dc-110"><a name ="singletunnel"></a>Single VPN tunnel</span></span>
<span data-ttu-id="b91dc-111">první topologie Hello se skládá z jednoho tunelu S2S VPN mezi bránu VPN Azure VPN a vaše místní zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="b91dc-111">hello first topology consists of a single S2S VPN tunnel between an Azure VPN gateway and your on-premises VPN device.</span></span> <span data-ttu-id="b91dc-112">Volitelně můžete nakonfigurovat protokol BGP mezi hello tunelového připojení sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="b91dc-112">You can optionally configure BGP across hello VPN tunnel.</span></span>

![jedno tunelové propojení](./media/vpn-gateway-3rdparty-device-config-overview/singletunnel.png)

<span data-ttu-id="b91dc-114">Odkazovat příliš[konfigurace připojení site-to-site](vpn-gateway-howto-site-to-site-resource-manager-portal.md) podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="b91dc-114">Refer too[Configure site-to-site connection](vpn-gateway-howto-site-to-site-resource-manager-portal.md) for detailed, step-by-step guidance.</span></span> <span data-ttu-id="b91dc-115">Hello následující části uvádějí hello parametry a zadejte toohelp skript prostředí PowerShell ukázkové, které začít pracovat.</span><span class="sxs-lookup"><span data-stu-id="b91dc-115">hello following sections list hello parameters and provide a sample PowerShell script toohelp you get started.</span></span>

### <a name="network-and-vpn-gateway-information"></a><span data-ttu-id="b91dc-116">Informace o bráně sítě a sítě VPN</span><span class="sxs-lookup"><span data-stu-id="b91dc-116">Network and VPN gateway information</span></span>
<span data-ttu-id="b91dc-117">V této části seznamu hello parametry pro výše uvedené příklady hello.</span><span class="sxs-lookup"><span data-stu-id="b91dc-117">This section list hello parameters for hello examples above.</span></span>

| <span data-ttu-id="b91dc-118">**Parametr**</span><span class="sxs-lookup"><span data-stu-id="b91dc-118">**Parameter**</span></span>                | <span data-ttu-id="b91dc-119">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="b91dc-119">**Value**</span></span>                    |
| ---                          | ---                          |
| <span data-ttu-id="b91dc-120">Předpony adres sítí VNet</span><span class="sxs-lookup"><span data-stu-id="b91dc-120">VNet address prefixes</span></span>        | <span data-ttu-id="b91dc-121">10.11.0.0/16</span><span class="sxs-lookup"><span data-stu-id="b91dc-121">10.11.0.0/16</span></span><br><span data-ttu-id="b91dc-122">10.12.0.0/16</span><span class="sxs-lookup"><span data-stu-id="b91dc-122">10.12.0.0/16</span></span> |
| <span data-ttu-id="b91dc-123">Azure IP adresa brány sítě VPN</span><span class="sxs-lookup"><span data-stu-id="b91dc-123">Azure VPN gateway IP</span></span>         | <span data-ttu-id="b91dc-124">Brána Azure VPN IP</span><span class="sxs-lookup"><span data-stu-id="b91dc-124">Azure VPN Gateway IP</span></span>         |
| <span data-ttu-id="b91dc-125">Předpony adres místní</span><span class="sxs-lookup"><span data-stu-id="b91dc-125">On-premises address prefixes</span></span> | <span data-ttu-id="b91dc-126">10.51.0.0/16</span><span class="sxs-lookup"><span data-stu-id="b91dc-126">10.51.0.0/16</span></span><br><span data-ttu-id="b91dc-127">10.52.0.0/16</span><span class="sxs-lookup"><span data-stu-id="b91dc-127">10.52.0.0/16</span></span> |
| <span data-ttu-id="b91dc-128">IP adresa zařízení místní sítě VPN</span><span class="sxs-lookup"><span data-stu-id="b91dc-128">On-premises VPN device IP</span></span>    | <span data-ttu-id="b91dc-129">IP adresa zařízení místní sítě VPN</span><span class="sxs-lookup"><span data-stu-id="b91dc-129">On-premises VPN device IP</span></span>    |
| <span data-ttu-id="b91dc-130">* Protokol BGP ASN virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="b91dc-130">*VNet BGP ASN</span></span>                | <span data-ttu-id="b91dc-131">65010</span><span class="sxs-lookup"><span data-stu-id="b91dc-131">65010</span></span>                        |
| <span data-ttu-id="b91dc-132">* Azure IP adresa partnera BGP</span><span class="sxs-lookup"><span data-stu-id="b91dc-132">*Azure BGP peer IP</span></span>           | <span data-ttu-id="b91dc-133">10.12.255.30</span><span class="sxs-lookup"><span data-stu-id="b91dc-133">10.12.255.30</span></span>                 |
| <span data-ttu-id="b91dc-134">* Místní ASN protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="b91dc-134">*On-premises BGP ASN</span></span>         | <span data-ttu-id="b91dc-135">65050</span><span class="sxs-lookup"><span data-stu-id="b91dc-135">65050</span></span>                        |
| <span data-ttu-id="b91dc-136">* IP adresa partnera BGP na místě</span><span class="sxs-lookup"><span data-stu-id="b91dc-136">*On-premises BGP peer IP</span></span>     | <span data-ttu-id="b91dc-137">10.52.255.254</span><span class="sxs-lookup"><span data-stu-id="b91dc-137">10.52.255.254</span></span>                |

* <span data-ttu-id="b91dc-138">(*) Volitelné parametry pro protokol BGP pouze</span><span class="sxs-lookup"><span data-stu-id="b91dc-138">(*) Optional parameters for BGP only</span></span>

### <a name="sample-powershell-script"></a><span data-ttu-id="b91dc-139">Ukázkový skript prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="b91dc-139">Sample PowerShell script</span></span>
<span data-ttu-id="b91dc-140">[Vytvoření připojení S2S VPN pomocí prostředí PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md) má hello podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="b91dc-140">[Create a S2S VPN connection using PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md) has hello detailed instructions.</span></span> <span data-ttu-id="b91dc-141">Tato část poskytuje ukázkové tooget skriptu, kterou jste zahájili.</span><span class="sxs-lookup"><span data-stu-id="b91dc-141">This section provides a sample script tooget you started.</span></span>

```powershell
# Declare your variables

$Sub1          = "Replace_With_Your_Subcription_Name"
$RG1           = "TestRG1"
$Location1     = "East US 2"
$VNetName1     = "TestVNet1"
$FESubName1    = "FrontEnd"
$BESubName1    = "Backend"
$GWSubName1    = "GatewaySubnet"
$VNetPrefix11  = "10.11.0.0/16"
$VNetPrefix12  = "10.12.0.0/16"
$FESubPrefix1  = "10.11.0.0/24"
$BESubPrefix1  = "10.12.0.0/24"
$GWSubPrefix1  = "10.12.255.0/27"
$VNet1ASN      = 65010
$DNS1          = "8.8.8.8"
$GWName1       = "VNet1GW"
$GWIPName1     = "VNet1GWIP"
$GWIPconfName1 = "gwipconf1"
$Connection15  = "VNet1toSite5"
$LNGName5      = "Site5"
$LNGPrefix50   = "10.52.255.254/32"
$LNGPrefix51   = "10.51.0.0/16"
$LNGPrefix52   = "10.52.0.0/16"
$LNGIP5        = "Your_VPN_Device_IP"
$LNGASN5       = 65050
$BGPPeerIP5    = "10.52.255.254"

# Connect tooyour subscription and create a new resource group

Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1

# Create virtual network

$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1 $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

# Create VPN gateway

$gwpip1    = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1     = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1

New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet1ASN

# Create local network gateway

New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix51,$LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5

# Create hello S2S VPN connection

$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $False
```

### <span data-ttu-id="b91dc-142"><a name ="policybased"></a>[Nepovinné] Pomocí vlastních zásad protokolu IPsec/IKE s "UsePolicyBasedTrafficSelectors"</span><span class="sxs-lookup"><span data-stu-id="b91dc-142"><a name ="policybased"></a> [Optional] Use custom IPsec/IKE policy with "UsePolicyBasedTrafficSelectors"</span></span>
<span data-ttu-id="b91dc-143">Pokud vaše zařízení VPN nepodporují selektory "any-to-any" provoz (na základě/VTI-založené na trasách konfigurace), budete potřebovat toocreate vlastní zásady protokolu IPsec/IKE a nakonfigurujte možnost "UsePolicyBasedTrafficSelectors", jak je popsáno v [v tomto článku ](vpn-gateway-connect-multiple-policybased-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="b91dc-143">If your VPN devices do not support "any-to-any" traffic selectors (route-based/VTI-based configuration), you will need toocreate a custom IPsec/IKE policy and configure "UsePolicyBasedTrafficSelectors" option as described in [this article](vpn-gateway-connect-multiple-policybased-rm-ps.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b91dc-144">Je nutné toocreate zásady protokolu IPsec/IKE v pořadí tooenable možnost "UsePolicyBasedTrafficSelectors" hello připojení.</span><span class="sxs-lookup"><span data-stu-id="b91dc-144">You need toocreate an IPsec/IKE policy in order tooenable "UsePolicyBasedTrafficSelectors" option on hello connection.</span></span>

<span data-ttu-id="b91dc-145">Ukázka skriptu Hello vytvoří zásadu protokolu IPsec/IKE se hello následující parametry a algoritmy:</span><span class="sxs-lookup"><span data-stu-id="b91dc-145">hello sample script below creates an IPsec/IKE policy with hello following algorithms and parameters:</span></span>
* <span data-ttu-id="b91dc-146">IKEv2: DHGroup24 AES256, SHA384</span><span class="sxs-lookup"><span data-stu-id="b91dc-146">IKEv2: AES256, SHA384, DHGroup24</span></span>
* <span data-ttu-id="b91dc-147">Protokol IPsec: AES256 SHA1, PFS24 7200 životnost přidružení zabezpečení sekund & 20480000KB (20GB)</span><span class="sxs-lookup"><span data-stu-id="b91dc-147">IPsec: AES256, SHA1, PFS24, SA Lifetime 7200 seconds & 20480000KB (20GB)</span></span>

<span data-ttu-id="b91dc-148">Poté použije hello zásad a umožňuje "UesPolicyBasedTrafficSelectors" hello připojení.</span><span class="sxs-lookup"><span data-stu-id="b91dc-148">It then applies hello policy and enables "UesPolicyBasedTrafficSelectors" on hello connection.</span></span>

```powershell
$ipsecpolicy5 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA1 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 20480000

$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $False -IpsecPolicies $ipsecpolicy5 -UsePolicyBasedTrafficSelectors $True
```

### <span data-ttu-id="b91dc-149"><a name ="bgp"></a>[Nepovinné] Použít protokol BGP na připojení S2S VPN</span><span class="sxs-lookup"><span data-stu-id="b91dc-149"><a name ="bgp"></a>[Optional] Use BGP on S2S VPN connection</span></span>
<span data-ttu-id="b91dc-150">Volitelně můžete BGP hello připojení.</span><span class="sxs-lookup"><span data-stu-id="b91dc-150">You can optionally use BGP on hello connection.</span></span> <span data-ttu-id="b91dc-151">V tématu [protokolu BGP pro bránu VPN](vpn-gateway-bgp-resource-manager-ps.md).</span><span class="sxs-lookup"><span data-stu-id="b91dc-151">See [BGP for VPN gateway](vpn-gateway-bgp-resource-manager-ps.md).</span></span> <span data-ttu-id="b91dc-152">Existují dva rozdíly:</span><span class="sxs-lookup"><span data-stu-id="b91dc-152">There are two differences:</span></span>

<span data-ttu-id="b91dc-153">předpony adres místní Hello může být adresa jednoho hostitele, adresa IP partnera BGP místní hello:</span><span class="sxs-lookup"><span data-stu-id="b91dc-153">hello on-premises address prefixes can be a single host address, hello on-premises BGP peer IP address:</span></span>

```powershell
New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
```

<span data-ttu-id="b91dc-154">Je nutné nastavit "-EnableBGP" příliš$ True při vytváření připojení hello:</span><span class="sxs-lookup"><span data-stu-id="b91dc-154">You must set "-EnableBGP" too$True when creating hello connection:</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

## <a name="next-steps"></a><span data-ttu-id="b91dc-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b91dc-155">Next steps</span></span>
<span data-ttu-id="b91dc-156">V tématu [konfiguraci brány VPN aktivní-aktivní pro mezi různými místy a připojení VNet-to-VNet](vpn-gateway-activeactive-rm-powershell.md) kroky tooconfigure aktivní aktivní mezi různými místy a připojení VNet-to-VNet.</span><span class="sxs-lookup"><span data-stu-id="b91dc-156">See [Configuring Active-Active VPN Gateways for Cross-Premises and VNet-to-VNet Connections](vpn-gateway-activeactive-rm-powershell.md) for steps tooconfigure active-active cross-premises and VNet-to-VNet connections.</span></span>

