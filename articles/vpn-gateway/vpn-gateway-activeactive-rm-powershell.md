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
# <a name="configure-active-active-s2s-vpn-connections-with-azure-vpn-gateways"></a>Konfigurace připojení S2S VPN aktivní aktivní službou Azure VPN Gateways

Tento článek vás provede kroky hello toocreate aktivní aktivní mezi různými místy a připojení VNet-to-VNet pomocí modelu nasazení Resource Manager hello a prostředí PowerShell.

## <a name="about-highly-available-cross-premises-connections"></a>Informace o připojeních mezi různými místy vysokou dostupností
Vysoká dostupnost tooachieve mezi různými místy a připojení VNet-to-VNet, doporučujeme nasadit více bran VPN a vytvořit více paralelních připojení mezi vaší sítí a Azure. Najdete v tématu [vysoce dostupné mezi různými místy a připojení VNet-to-VNet](vpn-gateway-highlyavailable.md) přehled možností připojení a topologii.

Tento článek obsahuje pokyny hello tooset až aktivní aktivní mezi různými místy, připojení VPN a aktivní aktivní připojení mezi dvěma virtuálními sítěmi:

* [Část 1 – Vytvoření a konfiguraci brány Azure VPN v režimu aktivní aktivní](#aagateway)
* [Část 2 – navázání připojení mezi různými místy aktivní aktivní](#aacrossprem)
* [Část 3 – vytvoření aktivní aktivní připojení VNet-to-VNet](#aav2v)
* [Část 4 - aktualizace mezi aktivní aktivní a aktivní pohotovostní existující bránu](#aaupdate)

Můžete kombinovat tyto společně toobuild složitější, vysoce dostupné síťové topologie, která vyhovuje vašim potřebám.

> [!IMPORTANT]
> Uvědomte si, že tento režim aktivní aktivní hello využívá jenom hello SKU následující: 
  * VpnGw1, VpnGw2, VpnGw3
  * HighPerformance (pro původní starší verze SKU)
> 
> 

## <a name ="aagateway"></a>Část 1 – Vytvoření a konfigurace aktivní aktivní brány sítě VPN
Následující kroky Hello nakonfiguruje bránu Azure VPN v režimech aktivní aktivní. Hello hlavní rozdíly mezi bránami aktivní aktivní a aktivní pohotovostní hello:

* Je třeba toocreate dvě konfigurace IP brány s dvě veřejné IP adresy
* Je nutné nastavit příznak EnableActiveActiveFeature hello
* Skladová položka brány Hello musí být VpnGw1 VpnGw2, VpnGw3 nebo HighPerformance (starší verze SKU).

Hello ostatní vlastnosti jsou hello stejné jako hello aktivní aktivní brány. 

### <a name="before-you-begin"></a>Než začnete
* Ověřte, že máte předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).
* Rutiny Powershellu pro Azure Resource Manager hello tooinstall budete potřebovat. V tématu [Přehled prostředí Azure PowerShell](/powershell/azure/overview) Další informace o instalaci rutin prostředí PowerShell hello.

### <a name="step-1---create-and-configure-vnet1"></a>Krok 1 – Vytvoření a konfigurace VNet1
#### <a name="1-declare-your-variables"></a>1. Deklarace proměnných
Tento ukázkový postup zahájíme deklarací proměnných. Následující příklad Hello deklaruje hello proměnné pomocí hello hodnot pro toto cvičení. Zda tooreplace hello hodnoty vlastními být při konfiguraci pro produkční prostředí. Tyto proměnné můžete použít, pokud používáte prostřednictvím toobecome kroky hello seznámili s tímto typem konfigurace. Umožňuje změnit proměnné hello a pak zkopírujte a vložte do konzoly prostředí PowerShell.

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

#### <a name="2-connect-tooyour-subscription-and-create-a-new-resource-group"></a>2. Připojení tooyour odběru a vytvořit novou skupinu prostředků
Ujistěte se, že jste přešli tooPowerShell režimu toouse hello rutiny správce prostředků. Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).

Otevřete konzolu prostředí PowerShell a připojte tooyour účtu. Použijte následující ukázka toohelp, ke kterým se připojujete hello:

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a>3. Vytvoření virtuální sítě TestVNet1
Hello následující ukázka vytvoří virtuální síť s názvem TestVNet1 a tři podsítě, jednu s názvem GatewaySubnet, jednu s názvem FrontEnd a jednu s názvem Backend. Při nahrazování hodnot je důležité vždy přiřadit podsíti brány konkrétní název GatewaySubnet. Pokud použijete jiný název, vytvoření brány se nezdaří.

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-hello-vpn-gateway-for-testvnet1-with-active-active-mode"></a>Krok 2 – Vytvoření brány VPN hello s režimem aktivní aktivní pro virtuální síť TestVNet1
#### <a name="1-create-hello-public-ip-addresses-and-gateway-ip-configurations"></a>1. Vytvoření hello veřejné IP adresy a konfigurace protokolu IP brány
Žádost o dvě veřejné IP adresy toobe přidělené toohello brány, které vytvoříte pro virtuální síť. Budete také definovat hello podsíť a požadované konfigurace protokolu IP.

```powershell
$gw1pip1 = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$gw1pip2 = New-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
$gw1ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2
```

#### <a name="2-create-hello-vpn-gateway-with-active-active-configuration"></a>2. Vytvoření brány VPN hello v konfiguraci aktivní aktivní
Vytvoření brány virtuální sítě hello pro virtuální síť TestVNet1. Upozorňujeme, že existují dvě položky GatewayIpConfig a hello EnableActiveActiveFeature příznak nastaven. Vytvoření brány může chvíli trvat (45 minut nebo další toocomplete).

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet1ASN -EnableActiveActiveFeature -Debug
```

#### <a name="3-obtain-hello-gateway-public-ip-addresses-and-hello-bgp-peer-ip-address"></a>3. Získat hello brány veřejné IP adresy a hello IP adresy partnera BGP
Po vytvoření brány hello budete potřebovat tooobtain hello IP adresy partnera BGP na hello Azure VPN Gateway. Tato adresa je potřebné tooconfigure hello Azure VPN Gateway jako partnerské zařízení protokolu BGP pro vaše místní zařízení VPN.

```powershell
$gw1pip1 = Get-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
$gw1pip2 = Get-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
```

Pomocí následující rutiny tooshow hello dvě veřejné IP adresy přidělené vaší brány sítě VPN a jejich odpovídající IP adresu partnera BGP pro každou instanci brány hello:

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

Hello pořadí hello veřejné IP adresy pro instance brány hello a hello, které jsou adresy odpovídající partnerského vztahu protokolu BGP hello stejné. V tomto příkladu hello brány virtuálních počítačů s veřejnou IP adresu z 40.112.190.5 použije 10.12.255.4 jako adresy partnerského vztahu protokolu BGP a brána hello s 138.91.156.129 bude používat 10.12.255.5. Tyto informace budete potřebovat při nastavování vašeho místního zařízení VPN připojení brány toohello aktivní aktivní. Hello brány je zobrazena v diagramu hello níže všechny adresy:

![aktivní aktivní brány](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

Po vytvoření brány hello můžete použít tuto bránu tooestablish aktivní aktivní mezi různými místy nebo připojení VNet-to-VNet. Následující části Hello provede hello kroky toocomplete hello cvičení.

## <a name ="aacrossprem"></a>Část 2 – Vytvoření připojení mezi různými místy aktivní aktivní
tooestablish připojení mezi různými místy, budete potřebovat toocreate bránu místní sítě toorepresent vaše místní zařízení VPN a brány Azure VPN připojení tooconnect hello s hello brány místní sítě. V tomto příkladu hello Azure VPN gateway je v režimu aktivní aktivní. V důsledku toho budou obě instance brány Azure VPN i v případě, že existuje pouze jeden místní zařízení VPN (brány místní sítě) a jedno připojení k prostředku, vytvořit tunelů S2S VPN s hello místního zařízení.

Než budete pokračovat, Zkontrolujte prosím, že jste dokončili [část 1](#aagateway) tohoto cvičení.

### <a name="step-1---create-and-configure-hello-local-network-gateway"></a>Krok 1 – Vytvoření a konfigurace brány místní sítě hello
#### <a name="1-declare-your-variables"></a>1. Deklarace proměnných
Toto cvičení bude pokračovat v konfiguraci hello toobuild vidět v diagramu hello. Být jisti tooreplace hello hodnoty hello ty, které jsou chcete toouse pro vaši konfiguraci.

```powershell
$RG5 = "TestAARG5"
$Location5 = "West US"
$LNGName51 = "Site5_1"
$LNGPrefix51 = "10.52.255.253/32"
$LNGIP51 = "131.107.72.22"
$LNGASN5 = 65050
$BGPPeerIP51 = "10.52.255.253"
```

Pár věcí toonote týkající se parametrů brány místní sítě hello:

* Brána místní sítě Hello může být v hello stejné nebo jiné umístění a prostředků skupiny jako hello brány VPN. Tento příklad ukazuje v různých skupinách prostředků, ale v hello stejného umístění Azure.
* Pokud existuje jenom jeden místní zařízení VPN jako v příkladu nahoře, hello aktivní aktivní připojení můžete pracovat s nebo bez protokolu BGP. Tento příklad používá protokol BGP pro připojení mezi různými místy hello.
* Pokud je protokol BGP povolený, předpona hello potřebujete toodeclare pro bránu místní sítě hello je adresa hostitele hello vaší IP adresy partnera BGP v zařízení VPN. V takovém případě je /32 předponu "10.52.255.253/32".
* Připomínáme je nutné použít různá čísla ASN protokolu BGP mezi vaší místní sítí a virtuální sítí VNet Azure. Pokud se jsou hello stejné, je třeba toochange vaší virtuální sítě ASN Pokud vaše místní zařízení VPN už používá hello ASN toopeer sousední ostatní směrovače BGP.

#### <a name="2-create-hello-local-network-gateway-for-site5"></a>2. Vytvoření brány místní sítě hello pro Site5
Než budete pokračovat, Zkontrolujte prosím, že jsou stále připojená tooSubscription 1. Vytvořte skupinu prostředků hello, pokud ještě není vytvořená.

```powershell
New-AzureRmResourceGroup -Name $RG5 -Location $Location5
New-AzureRmLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51
```

### <a name="step-2---connect-hello-vnet-gateway-and-local-network-gateway"></a>Krok 2 – připojit hello brány virtuální sítě a brány místní sítě
#### <a name="1-get-hello-two-gateways"></a>1. Získat hello dvě brány

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw1 = Get-AzureRmLocalNetworkGateway  -Name $LNGName51 -ResourceGroupName $RG5
```

#### <a name="2-create-hello-testvnet1-toosite5-connection"></a>2. Vytvoření připojení tooSite5 hello virtuální sítě TestVNet1
V tomto kroku vytvoříte hello připojení z virtuální sítě TestVNet1 tooSite5_1 s "EnableBGP" nastavit také$ True.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-on-premises-vpn-device"></a>3. Připojení VPN a BGP parametry pro vaše místní zařízení VPN
Následující příklad Hello uvádí hello parametry, které budete zadávat do hello BGP konfigurační oddíl na vaše místní zařízení VPN pro toto cvičení:

    - Site5 číslo ASN: 65050
    - IP protokol BGP Site5: 10.52.255.253
    - Předpony tooannounce: (například) 10.51.0.0/16 a 10.52.0.0/16
    - Azure VNet číslo ASN: 65010
    - Azure VNet IP protokolu BGP 1: 10.12.255.4 pro too40.112.190.5 tunelového propojení
    - Azure VNet IP protokolu BGP 2: 10.12.255.5 pro too138.91.156.129 tunelového propojení
    - Statické trasy: cílový 10.12.255.4/32, dalšího segmentu hello VPN tunelového propojení rozhraní too40.112.190.5 cílové 10.12.255.5/32, dalšího segmentu hello VPN tunelového propojení rozhraní too138.91.156.129
    - eBGP pokus: Zajistěte možnost "pokus" hello, eBGP je povolena na vašem zařízení, v případě potřeby

Hello připojení by se mělo vytvořit po několik minut a hello relaci partnerského vztahu protokolu BGP se spustí po vytvoření hello připojení protokolu IPsec. Tento příklad, pokud byl nakonfigurován pouze jeden zařízení VPN místní, což vede k hello diagramu níže:

![aktivní aktivní crossprem](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### <a name="step-3---connect-two-on-premises-vpn-devices-toohello-active-active-vpn-gateway"></a>Krok 3 – připojení dvě místní sítě VPN zařízení toohello aktivní aktivní brány VPN
Pokud máte dva zařízení VPN na hello stejné místní síti, můžete dosáhnout duální redundance připojování hello Azure VPN gateway toohello druhé zařízení VPN.

#### <a name="1-create-hello-second-local-network-gateway-for-site5"></a>1. Vytvoření brány místní sítě druhý hello pro Site5
Všimněte si, že IP adresa brány hello, předponu adresy a adresu partnerského vztahu BGP pro bránu místní sítě druhý hello se nesmí překrývat s hello předchozí brána místní sítě pro hello stejné místní síti.

```powershell
$LNGName52 = "Site5_2"
$LNGPrefix52 = "10.52.255.254/32"
$LNGIP52 = "131.107.72.23"
$BGPPeerIP52 = "10.52.255.254"

New-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
```

#### <a name="2-connect-hello-vnet-gateway-and-hello-second-local-network-gateway"></a>2. Připojení brány virtuální sítě hello a druhá brána místní sítě hello
Vytvoření hello připojení z virtuální sítě TestVNet1 tooSite5_2 s "EnableBGP" nastavit také$ True

```powershell
$lng5gw2 = Get-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-second-on-premises-vpn-device"></a>3. Připojení VPN a BGP parametry pro druhý místní zařízení VPN
Níže uvedené parametry hello seznamy Podobně bude zadejte do druhé zařízení VPN hello:

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

Jakmile se navázat připojení hello (tunely), budete mít dva redundantní zařízení VPN a tunely připojení vaší místní sítí a Azure:

![Dual redundance crossprem](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)

## <a name ="aav2v"></a>Část 3 – vytvoření aktivní aktivní připojení VNet-to-VNet
V této části vytvoříme připojení k aktivní aktivní VNet-to-VNet s protokolem BGP. 

níže uvedené pokyny Hello pokračovat od hello kroků uvedených výše. Je třeba provést [část 1](#aagateway) toocreate a konfigurace virtuální sítě TestVNet1 a hello brány VPN s protokolem BGP. 

### <a name="step-1---create-testvnet2-and-hello-vpn-gateway"></a>Krok 1 – Vytvoření brány VPN TestVNet2 a hello
Je důležité toomake jistotu, že hello adresní prostor IP adres hello nové virtuální sítě, TestVNet2, nepřekrývá s žádným z rozsahů vaší virtuální sítě.

V tomto příkladu patří virtuální sítě hello toohello stejného předplatného. Můžete nastavit připojení VNet-to-VNet mezi různých předplatných; naleznete příliš[konfigurace připojení typu VNet-to-VNet](vpn-gateway-vnet-vnet-rm-ps.md) toolearn více podrobností. Zajistěte, aby přidáte hello "-EnableBgp $True" při vytváření hello tooenable připojení protokolu BGP.

#### <a name="1-declare-your-variables"></a>1. Deklarace proměnných
Být jisti tooreplace hello hodnoty hello ty, které jsou chcete toouse pro vaši konfiguraci.

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

#### <a name="2-create-testvnet2-in-hello-new-resource-group"></a>2. Vytvoření TestVNet2 v hello novou skupinu prostředků

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2
```

#### <a name="3-create-hello-active-active-vpn-gateway-for-testvnet2"></a>3. Vytvoření brány VPN hello aktivní aktivní pro TestVNet2
Žádost o dvě veřejné IP adresy toobe přidělené toohello brány, které vytvoříte pro virtuální síť. Budete také definovat hello podsíť a požadované konfigurace protokolu IP.

```powershell
$gw2pip1 = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
$gw2pip2 = New-AzureRmPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2 = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
$gw2ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2
```

Vytvoření brány VPN hello s hello jako číslo a hello příznak "EnableActiveActiveFeature". Poznámka: na Azure VPN Gateway, je nutné přepsat hello výchozí číslo ASN. Hello čísla ASN pro hello připojené virtuální sítě musí být jiný tooenable protokolu BGP a směrování přenosu.

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet2ASN -EnableActiveActiveFeature
```

### <a name="step-2---connect-hello-testvnet1-and-testvnet2-gateways"></a>Krok 2 – připojení brány virtuální sítě TestVNet1 a TestVNet2 hello
V tomto příkladu jsou obě brány v hello stejného předplatného. Můžete použít tento krok v hello stejné relace prostředí PowerShell.

#### <a name="1-get-both-gateways"></a>1. Získat obě brány
Ujistěte se, přihlásit a připojte tooSubscription 1.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
```

#### <a name="2-create-both-connections"></a>2. Vytvoření obě připojení
V tomto kroku vytvoříte hello připojení z virtuální sítě TestVNet1 tooTestVNet2 a hello připojení z TestVNet2 tooTestVNet1.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> Být jisti tooenable protokolu BGP pro obě připojení.
> 
> 

Po dokončení těchto kroků, připojení hello navážou za pár minut a hello relaci partnerského vztahu protokolu BGP bude až po dokončení připojení hello VNet-to-VNet s duální redundance:

![aktivní aktivní v2v](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <a name ="aaupdate"></a>Část 4 - aktualizace mezi aktivní aktivní a aktivní pohotovostní existující bránu
poslední část Hello popíše, jak můžete nakonfigurovat existující bránu Azure VPN z aktivní pohotovostní režim tooactive aktivní, nebo naopak.

> [!NOTE]
> Tato část obsahuje kroky tooresize hello starší verze skladové jednotky (SKU staré) již vytvořené brány VPN ze standardní tooHighPerformance. Tyto kroky neupgradovat původní starší verze SKU po tooone hello nové SKU.
> 
> 

### <a name="configure-an-active-standby-gateway-tooactive-active-gateway"></a>Konfigurace brány tooactive aktivní aktivní pohotovostní brány
#### <a name="1-gateway-parameters"></a>1. Parametry brány
Následující ukázka Hello převede bránu aktivní pohotovostní bránu aktivní aktivní. Třeba toocreate jinou veřejnou IP adresu a pak přidejte druhý konfigurace IP brány. Níže znázorňuje hello používají parametry:

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

#### <a name="2-create-hello-public-ip-address-then-add-hello-second-gateway-ip-configuration"></a>2. Vytvoření hello veřejnou IP adresu a pak přidejte druhý konfigurace IP brány hello

```powershell
$gwpip2 = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
Add-AzureRmVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2
```

#### <a name="3-enable-active-active-mode-and-update-hello-gateway"></a>3. Povolit aktivní aktivní režim a aktualizace hello brány
Objekt hello brány musí být nastavena v prostředí PowerShell tootrigger hello skutečné aktualizace. Hello skladová položka brány virtuální sítě hello musí být také změněno (změněnou) tooHighPerformance vzhledem k tomu, že byla dříve vytvořena jako Standard.

```powershell
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature -GatewaySku HighPerformance
```

Tato aktualizace může trvat 30 minut too45.

### <a name="configure-an-active-active-gateway-tooactive-standby-gateway"></a>Konfigurace brány tooactive pohotovostní aktivní aktivní brány
#### <a name="1-gateway-parameters"></a>1. Parametry brány
Použití hello stejné parametry, jak je uvedeno výše, získat název hello konfigurace protokolu IP hello chcete tooremove.

```powershell
$GWName = "TestVNetAA1GW"
$RG = "TestVPNActiveActive01"

$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
$ipconfname = $gw.IpConfigurations[1].Name
```

#### <a name="2-remove-hello-gateway-ip-configuration-and-disable-hello-active-active-mode"></a>2. Odeberte konfigurace IP brány hello a zakázat režim aktivní aktivní hello
Podobně je nutné nastavit hello brány objekt v prostředí PowerShell tootrigger hello skutečné aktualizace.

```powershell
Remove-AzureRmVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature
```

Tato aktualizace může trvat až too30 příliš 45 minut.

## <a name="next-steps"></a>Další kroky
Po dokončení připojení můžete přidat virtuální počítače tooyour virtuální sítě. Kroky jsou uvedeny v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
