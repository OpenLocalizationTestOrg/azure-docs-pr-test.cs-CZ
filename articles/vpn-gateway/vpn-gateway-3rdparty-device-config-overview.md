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
# <a name="overview-of-3rd-party-vpn-device-configurations"></a>Přehled o 3. konfigurace zařízení VPN strany
Tento článek obsahuje přehled místní konfigurace zařízení VPN pro připojení brány VPN tooAzure. Ukázka Hello virtuální síť Azure a bude instalační program brány sítě VPN se stejnými parametry použité tooconnect toodifferent místní zařízení VPN s hello.

## <a name="device-requirements"></a>Požadavky na zařízení
Azure VPN Gateway pomocí standardní sady protokolu IPsec/IKE pro tunelů S2S VPN. Odkazovat příliš[informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md) hello podrobné parametry protokolu IPsec/IKE a výchozí kryptografické algoritmy pro Azure VPN Gateway. Volitelně můžete zadat přesný kombinace hello kryptografické algoritmy a klíče síly pro konkrétní připojení jak je popsáno v [o požadavcích na kryptografických](vpn-gateway-about-compliance-crypto.md).

## <a name ="singletunnel"></a>Jedno tunelové propojení sítě VPN
první topologie Hello se skládá z jednoho tunelu S2S VPN mezi bránu VPN Azure VPN a vaše místní zařízení VPN. Volitelně můžete nakonfigurovat protokol BGP mezi hello tunelového připojení sítě VPN.

![jedno tunelové propojení](./media/vpn-gateway-3rdparty-device-config-overview/singletunnel.png)

Odkazovat příliš[konfigurace připojení site-to-site](vpn-gateway-howto-site-to-site-resource-manager-portal.md) podrobné pokyny. Hello následující části uvádějí hello parametry a zadejte toohelp skript prostředí PowerShell ukázkové, které začít pracovat.

### <a name="network-and-vpn-gateway-information"></a>Informace o bráně sítě a sítě VPN
V této části seznamu hello parametry pro výše uvedené příklady hello.

| **Parametr**                | **Hodnota**                    |
| ---                          | ---                          |
| Předpony adres sítí VNet        | 10.11.0.0/16<br>10.12.0.0/16 |
| Azure IP adresa brány sítě VPN         | Brána Azure VPN IP         |
| Předpony adres místní | 10.51.0.0/16<br>10.52.0.0/16 |
| IP adresa zařízení místní sítě VPN    | IP adresa zařízení místní sítě VPN    |
| * Protokol BGP ASN virtuální sítě                | 65010                        |
| * Azure IP adresa partnera BGP           | 10.12.255.30                 |
| * Místní ASN protokolu BGP         | 65050                        |
| * IP adresa partnera BGP na místě     | 10.52.255.254                |

* (*) Volitelné parametry pro protokol BGP pouze

### <a name="sample-powershell-script"></a>Ukázkový skript prostředí PowerShell
[Vytvoření připojení S2S VPN pomocí prostředí PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md) má hello podrobné pokyny. Tato část poskytuje ukázkové tooget skriptu, kterou jste zahájili.

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

### <a name ="policybased"></a>[Nepovinné] Pomocí vlastních zásad protokolu IPsec/IKE s "UsePolicyBasedTrafficSelectors"
Pokud vaše zařízení VPN nepodporují selektory "any-to-any" provoz (na základě/VTI-založené na trasách konfigurace), budete potřebovat toocreate vlastní zásady protokolu IPsec/IKE a nakonfigurujte možnost "UsePolicyBasedTrafficSelectors", jak je popsáno v [v tomto článku ](vpn-gateway-connect-multiple-policybased-rm-ps.md).

> [!IMPORTANT]
> Je nutné toocreate zásady protokolu IPsec/IKE v pořadí tooenable možnost "UsePolicyBasedTrafficSelectors" hello připojení.

Ukázka skriptu Hello vytvoří zásadu protokolu IPsec/IKE se hello následující parametry a algoritmy:
* IKEv2: DHGroup24 AES256, SHA384
* Protokol IPsec: AES256 SHA1, PFS24 7200 životnost přidružení zabezpečení sekund & 20480000KB (20GB)

Poté použije hello zásad a umožňuje "UesPolicyBasedTrafficSelectors" hello připojení.

```powershell
$ipsecpolicy5 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA1 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 20480000

$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $False -IpsecPolicies $ipsecpolicy5 -UsePolicyBasedTrafficSelectors $True
```

### <a name ="bgp"></a>[Nepovinné] Použít protokol BGP na připojení S2S VPN
Volitelně můžete BGP hello připojení. V tématu [protokolu BGP pro bránu VPN](vpn-gateway-bgp-resource-manager-ps.md). Existují dva rozdíly:

předpony adres místní Hello může být adresa jednoho hostitele, adresa IP partnera BGP místní hello:

```powershell
New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
```

Je nutné nastavit "-EnableBGP" příliš$ True při vytváření připojení hello:

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

## <a name="next-steps"></a>Další kroky
V tématu [konfiguraci brány VPN aktivní-aktivní pro mezi různými místy a připojení VNet-to-VNet](vpn-gateway-activeactive-rm-powershell.md) kroky tooconfigure aktivní aktivní mezi různými místy a připojení VNet-to-VNet.

