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
# <a name="how-tooconfigure-bgp-on-azure-vpn-gateways-using-powershell"></a>Jak tooconfigure BGP na Azure VPN Gateway pomocí prostředí PowerShell
Tento článek vás provede kroky tooenable hello protokolu BGP na připojení Site-to-Site (S2S) VPN mezi různými místy a připojení VNet-to-VNet pomocí modelu nasazení Resource Manager hello a prostředí PowerShell.

## <a name="about-bgp"></a>Informace o protokolu BGP
BGP je standardní směrovací protokol hello běžně používaný v hello Internet tooexchange směrování a dostupnosti informací mezi dvěma nebo více sítěmi. Protokol BGP umožňuje hello Azure VPN Gateway a vaše místní zařízení VPN, nazývají partnerské uzly protokolu BGP nebo Sousedé BGP, tooexchange "tras", bude informovat obě brány o dostupnosti hello a dostupnosti pro ty předpony toogo prostřednictvím hello bránami nebo trasami související se situací. Protokol BGP můžete také povolit směrování přenosu mezi více sítěmi pomocí šíření tras, které brána s protokolem BGP zjistí od jednoho tooall partnera BGP ostatním partnerům BGP.

V tématu [přehled protokolu BGP službou Azure VPN Gateways](vpn-gateway-bgp-overview.md) pro další zabývat výhody protokol BGP a toounderstand hello technické požadavky a důležité informace o použití protokolu BGP.

## <a name="getting-started-with-bgp-on-azure-vpn-gateways"></a>Začínáme s protokolem BGP na branách Azure VPN

Tento článek vás provede hello kroky toodo hello následující úlohy:

* [Část 1 - povolit protokol BGP na bráně Azure VPN](#enablebgp)
* [Část 2 – navázání připojení mezi různými místy pomocí protokolu BGP](#crossprembgp)
* [Část 3 – připojení VNet-to-VNet s protokolem BGP](#v2vbgp)

Jednotlivých součástí hello pokyny tvoří základní stavební blok pro povolení protokolu BGP v připojení k síti. Pokud dokončíte všechny tři části, jak je znázorněno v následujícím diagramu hello sestavení hello topologie:

![Topologii BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

Můžete kombinovat částí společně toobuild složitější, vícenásobného předávání tranzitní sítě, která vyhovuje vašim potřebám.

## <a name ="enablebgp"></a>Část 1: Konfigurace protokolu BGP na hello Azure VPN Gateway
Postup konfigurace Hello nastavit hello parametry protokolu BGP brány Azure VPN hello jak je znázorněno v následujícím diagramu hello:

![Protokol BGP brány](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a>Než začnete
* Ověřte, že máte předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).
* Nainstalujte rutiny Powershellu pro Azure Resource Manager hello. Další informace o instalaci rutin prostředí PowerShell hello najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview). 

### <a name="step-1---create-and-configure-vnet1"></a>Krok 1 – Vytvoření a konfigurace VNet1
#### <a name="1-declare-your-variables"></a>1. Deklarace proměnných
Pro toto cvičení začneme deklarací proměnných. Hello následující příklad deklaruje hello proměnné pomocí hello hodnot pro toto cvičení. Zda tooreplace hello hodnoty vlastními být při konfiguraci pro produkční prostředí. Tyto proměnné můžete použít, pokud používáte prostřednictvím toobecome kroky hello seznámili s tímto typem konfigurace. Umožňuje změnit proměnné hello a pak zkopírujte a vložte do konzoly prostředí PowerShell.

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

#### <a name="2-connect-tooyour-subscription-and-create-a-new-resource-group"></a>2. Připojení tooyour odběru a vytvořit novou skupinu prostředků
toouse hello rutiny Resource Manager, ujistěte se, že jste přešli tooPowerShell režimu. Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).

Otevřete konzolu prostředí PowerShell a připojte tooyour účtu. Použijte následující ukázka toohelp, ke kterým se připojujete hello:

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a>3. Vytvoření virtuální sítě TestVNet1
Hello následující ukázka vytvoří virtuální síť s názvem TestVNet1 a tři podsítě, jednu s názvem GatewaySubnet, jednu s názvem FrontEnd a jednu s názvem Backend. Při nahrazování hodnot je důležité vždy přiřadit podsíti brány konkrétní název GatewaySubnet. Pokud použijete jiný název, vytvoření brány se nezdaří.

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1 $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-hello-vpn-gateway-for-testvnet1-with-bgp-parameters"></a>Krok 2 – Vytvoření hello brána sítě VPN pro virtuální síť TestVNet1 s parametry protokolu BGP
#### <a name="1-create-hello-ip-and-subnet-configurations"></a>1. Vytvoření hello konfigurací IP adresy a podsítě
Požádat o veřejné IP adresy toobe přidělené toohello bránu, které vytvoříte pro virtuální síť. Budete také definovat hello vyžaduje podsíť a konfigurace protokolu IP.

```powershell
$gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1
```

#### <a name="2-create-hello-vpn-gateway-with-hello-as-number"></a>2. Vytvoření brány VPN hello s hello jako číslo
Vytvoření brány virtuální sítě hello pro virtuální síť TestVNet1. Protokol BGP vyžaduje Trasové brány sítě VPN a také hello přidání parametru - číslo Asn, tooset hello ASN (čísla AS) pro virtuální síť TestVNet1. Pokud parametr číslo ASN hello nenastavíte, bude přiřazeno číslo ASN 65515. Vytvoření brány může chvíli trvat (30 minut nebo další toocomplete).

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN
```

#### <a name="3-obtain-hello-azure-bgp-peer-ip-address"></a>3. Získat adresu IP adresa partnera BGP Azure hello
Po vytvoření brány hello musíte tooobtain hello IP adresy partnera BGP na hello Azure VPN Gateway. Tato adresa je potřebné tooconfigure hello Azure VPN Gateway jako partnerské zařízení protokolu BGP pro vaše místní zařízení VPN.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet1gw.BgpSettingsText
```

poslední příkaz Hello zobrazí odpovídající konfigurace protokolu BGP hello na hello Azure VPN Gateway; například:

```powershell
$vnet1gw.BgpSettingsText
{
    "Asn": 65010,
    "BgpPeeringAddress": "10.12.255.30",
    "PeerWeight": 0
}
```

Po vytvoření brány hello lze použít toto připojení mezi různými místy tooestablish brány nebo připojení VNet-to-VNet s protokolem BGP. Hello následující části provede hello kroky toocomplete hello cvičení.

## <a name ="crossprembbgp"></a>Část 2 – navázání připojení mezi různými místy pomocí protokolu BGP

tooestablish připojení mezi různými místy, budete potřebovat toocreate toorepresent bránu místní sítě vašeho místního zařízení VPN a brány VPN připojení tooconnect hello s hello brány místní sítě. Když jsou články, které vás provedou postupem, tento článek obsahuje parametry konfigurace protokolu BGP požadované toospecify hello aplikace hello další vlastnosti.

![Protokol BGP pro více míst](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

Než budete pokračovat, ujistěte se, když jste dokončili [část 1](#enablebgp) tohoto cvičení.

### <a name="step-1---create-and-configure-hello-local-network-gateway"></a>Krok 1 – Vytvoření a konfigurace brány místní sítě hello

#### <a name="1-declare-your-variables"></a>1. Deklarace proměnných

Toto cvičení pokračuje toobuild hello konfigurace znázorněné hello diagram. Být jisti tooreplace hello hodnoty hello ty, které jsou chcete toouse pro vaši konfiguraci.

```powershell
$RG5 = "TestBGPRG5"
$Location5 = "East US 2"
$LNGName5 = "Site5"
$LNGPrefix50 = "10.52.255.254/32"
$LNGIP5 = "Your_VPN_Device_IP"
$LNGASN5 = 65050
$BGPPeerIP5 = "10.52.255.254"
```

Pár věcí toonote týkající se parametrů brány místní sítě hello:

* Brána místní sítě Hello může být v hello stejné nebo jiné umístění a prostředků skupiny jako hello brány VPN. Tento příklad ukazuje je v různých skupinách prostředků v různých umístěních.
* minimální předponu Hello potřebujete toodeclare pro bránu místní sítě hello je adresa hostitele hello vaší IP adresy partnera BGP v zařízení VPN. V takovém případě je /32 předponu "10.52.255.254/32".
* Připomínáme je nutné použít různá čísla ASN protokolu BGP mezi vaší místní sítí a virtuální sítí VNet Azure. Pokud se jsou hello stejné, je třeba toochange vaší virtuální sítě ASN Pokud vaše místní zařízení VPN už používá hello ASN toopeer sousední ostatní směrovače BGP.

Než budete pokračovat, ujistěte se, že jsou stále připojená tooSubscription 1.

#### <a name="2-create-hello-local-network-gateway-for-site5"></a>2. Vytvoření brány místní sítě hello pro Site5

Být zda skupiny prostředků toocreate hello, pokud není vytvořená, před vytvořením brány místní sítě hello. Všimněte si hello dva další parametry pro bránu místní sítě hello: číslo Asn a BgpPeerAddress.

```powershell
New-AzureRmResourceGroup -Name $RG5 -Location $Location5

New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
```

### <a name="step-2---connect-hello-vnet-gateway-and-local-network-gateway"></a>Krok 2 – připojit hello brány virtuální sítě a brány místní sítě

#### <a name="1-get-hello-two-gateways"></a>1. Získat hello dvě brány

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5
```

#### <a name="2-create-hello-testvnet1-toosite5-connection"></a>2. Vytvoření připojení tooSite5 hello virtuální sítě TestVNet1

V tomto kroku vytvoříte hello připojení z virtuální sítě TestVNet1 tooSite5. Je nutné zadat "-EnableBGP $True" tooenable protokolu BGP pro toto připojení. Jak už jsme probírali výše, je možné toohave připojení protokolu BGP i bez protokolu BGP pro hello stejné Azure VPN Gateway. Pokud je protokol BGP povolený ve vlastnosti připojení hello, neumožní Azure pro toto připojení protokolu BGP, i když BGP parametry jsou již nakonfigurované na obě brány.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

Hello následující příklad obsahuje hello parametry, které zadáte do hello BGP konfigurační oddíl na vaše místní zařízení VPN pro tento postup:

```

- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes tooannounce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP    : 10.12.255.30
- Static route         : Add a route for 10.12.255.30/32, with nexthop being hello VPN tunnel interface on your device
- eBGP Multihop        : Ensure hello "multihop" option for eBGP is enabled on your device if needed
```

Hello připojení po několik minut a hello BGP partnerského vztahu relace spustí po vytvoření hello připojení protokolu IPsec.

## <a name ="v2vbgp"></a>Část 3 – připojení VNet-to-VNet s protokolem BGP

Tato část přidá připojení VNet-to-VNet s protokolem BGP, jak je znázorněno v následujícím diagramu hello:

![Protokol BGP pro síť VNet-to-VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

Hello pokynů pokračovat z předchozích kroků hello. Je třeba provést [části I](#enablebgp) toocreate a konfigurace virtuální sítě TestVNet1 a hello brány VPN s protokolem BGP. 

### <a name="step-1---create-testvnet2-and-hello-vpn-gateway"></a>Krok 1 – Vytvoření brány VPN TestVNet2 a hello

Je důležité toomake jistotu, že hello adresní prostor IP adres hello nové virtuální sítě, TestVNet2, nepřekrývá s žádným z rozsahů vaší virtuální sítě.

V tomto příkladu patří virtuální sítě hello toohello stejného předplatného. Můžete nastavit připojení VNet-to-VNet mezi různých předplatných. Další informace najdete v tématu [konfigurace připojení typu VNet-to-VNet](vpn-gateway-vnet-vnet-rm-ps.md). Zajistěte, aby přidáte hello "-EnableBgp $True" při vytváření hello tooenable připojení protokolu BGP.

#### <a name="1-declare-your-variables"></a>1. Deklarace proměnných

Být jisti tooreplace hello hodnoty hello ty, které jsou chcete toouse pro vaši konfiguraci.

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

#### <a name="2-create-testvnet2-in-hello-new-resource-group"></a>2. Vytvoření TestVNet2 v hello novou skupinu prostředků

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2
```

#### <a name="3-create-hello-vpn-gateway-for-testvnet2-with-bgp-parameters"></a>3. Vytvoření brány VPN hello pro TestVNet2 s parametry protokolu BGP

Požádat o veřejné IP adresy toobe přidělené toohello bránu vytvoříte pro virtuální sítě a definujte hello vyžaduje podsíť a konfigurace protokolu IP.

```powershell
$gwpip2    = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2     = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gwipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2
```

Vytvoření brány VPN hello s hello jako číslo. Na Azure VPN Gateway je nutné přepsat hello výchozí číslo ASN. Hello čísla ASN pro hello připojené virtuální sítě musí být jiný tooenable protokolu BGP a směrování přenosu.

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard -Asn $VNet2ASN
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

V tomto kroku vytvoříte z TestVNet2 tooTestVNet1 hello připojení z virtuální sítě TestVNet1 tooTestVNet2 a hello připojení.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> Být jisti tooenable protokolu BGP pro obě připojení.
> 
> 

Po dokončení těchto kroků, hello připojení po několika minutách. relaci partnerského vztahu protokolu BGP Hello je až po dokončení hello připojení VNet-to-VNet.

Pokud jste dokončili všechny tři části tohoto cvičení, jste vytvořili hello následující topologie sítě:

![Protokol BGP pro síť VNet-to-VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## <a name="next-steps"></a>Další kroky

Po dokončení připojení můžete přidat virtuální počítače tooyour virtuální sítě. Kroky jsou uvedeny v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
