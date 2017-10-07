---
title: "Konfigurace ExpressRoute a připojení VPN typu site-to-site, která mohou existovat vedle sebe: Resource Manager: Azure | Dokumentace Microsoftu"
description: "Tento článek vás provede konfigurací ExpressRoute a připojení VPN typu site-to-site, která mohou v modelu nasazení Resource Manager existovat vedle sebe."
documentationcenter: na
services: expressroute
author: charwen
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7717b14-3da3-4a6d-b78e-a5020766bc2c
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/19/2017
ms.author: charwen,cherylmc
ms.openlocfilehash: efda9f89d95617c8c4e75af91b20631dc468d4db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-expressroute-and-site-to-site-coexisting-connections"></a>Konfigurace společně používaných připojení typu Site-to-Site a ExpressRoute
> [!div class="op_single_selector"]
> * [PowerShell – Resource Manager](expressroute-howto-coexist-resource-manager.md)
> * [PowerShell – Classic](expressroute-howto-coexist-classic.md)
> 
> 

Konfigurace ExpressRoute a současně existujících připojení VPN typu Site-to-Site má několik výhod. Můžete nakonfigurovat VPN typu Site-to-Site jako cestu zabezpečené převzetí služeb při selhání pro ExressRoute, nebo použít toosites tooconnect sítě Site-to-Site VPN, které nejsou připojené prostřednictvím ExpressRoute. Nabídneme tooconfigure kroky hello oba scénáře v tomto článku. Tento článek vztahuje toohello modelu nasazení Resource Manager a používá prostředí PowerShell. Tato konfigurace není k dispozici v hello portálu Azure.

> [!IMPORTANT]
> Okruhy ExpressRoute musí být předem nakonfigurované, než začnete postupovat podle pokynů hello. Ujistěte se, že jste postupovali podle příručky hello příliš[vytvoření okruhu ExpressRoute](expressroute-howto-circuit-arm.md) a [konfigurace směrování](expressroute-howto-routing-arm.md) než budete pokračovat.
> 
> 

## <a name="limits-and-limitations"></a>Omezení
* **Směrování provozu není podporováno.** Nemůžete provádět směrování (přes Azure) mezi místní sítí připojenou prostřednictvím sítě VPN typu site-to-site a místní sítí připojenou přes ExpressRoute.
* **Základní brána SKU není podporována.** Musíte použít bránu bez – základní SKU pro obě hello [brány ExpressRoute](expressroute-about-virtual-network-gateways.md) a hello [brány VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md).
* **Podporována je pouze brána VPN na základě tras.** Je nutné použít službu [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) na základě tras.
* **Pro vaši bránu VPN by měla být nakonfigurována statická trasa.** Pokud vaše místní síť připojená tooboth ExpressRoute a Site-to-Site VPN, musíte mít ve vaší místní síti tooroute hello Site-to-Site VPN připojení toohello konfigurovanou statickou trasu veřejného Internetu.
* **Bránu ExpressRoute musí být nakonfigurovaná a propojit tooa okruh.** Je nutné nejprve vytvořte bránu ExpressRoute hello a tu propojit tooa okruhu předtím, než přidáte bránu VPN hello Site-to-Site.

## <a name="configuration-designs"></a>Návrhy konfigurace
### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a>Konfigurace VPN typu site-to-site jako cesty převzetí služeb při selhání pro ExpressRoute
Můžete nakonfigurovat připojení VPN typu site-to-site jako záložní pro ExpressRoute. To platí pouze toovirtual sítě propojené toohello cestou soukromého partnerského vztahu Azure. Neexistuje žádné řešení převzetí služeb při selhání založené na VPN pro služby, které jsou přístupné prostřednictvím veřejného partnerského vztahu Azure nebo partnerského vztahu Microsoftu. Hello okruh ExpressRoute je vždy hello primární propojení. Data proudí prostřednictvím cesty hello Site-to-Site VPN, pouze pokud hello okruh ExpressRoute selže.

> [!NOTE]
> Při okruh ExpressRoute je upřednostňované prostřednictvím sítě Site-to-Site VPN, když oba trasy jsou hello stejné, použije Azure hello nejdelší předponu shodu toochoose hello trasy směrem cíle hello paketu.
> 
> 

![Současná existence](media/expressroute-howto-coexist-resource-manager/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-tooconnect-toosites-not-connected-through-expressroute"></a>Konfigurace Site-to-Site VPN tooconnect toosites nejsou připojené prostřednictvím ExpressRoute
Můžete nakonfigurovat síti, kde některé weby jsou připojené přímo tooAzure prostřednictvím sítě Site-to-Site VPN a některé weby přes ExpressRoute. 

![Současná existence](media/expressroute-howto-coexist-resource-manager/scenario2.jpg)

> [!NOTE]
> Virtuální síť nemůžete nakonfigurovat jako směrovač provozu.
> 
> 

## <a name="selecting-hello-steps-toouse"></a>Výběr toouse kroky hello
Existují dvě sady postupů toochoose z. Postup konfigurace Hello, který vyberete, závisí na tom, jestli máte existující virtuální síť, které mají tooconnect k, nebo chcete toocreate nové virtuální sítě.

* Nemám virtuální síť a potřebuji toocreate jeden.
  
    Pokud ještě nemáte virtuální síť, tento postup vás provede procesem vytvoření nové virtuální sítě pomocí modelu nasazení Resource Manager a vytvoření nových připojení ExpressRoute a VPN typu Site-to-Site. tooconfigure virtuální sítě, postupujte podle kroků hello v [toocreate nové virtuální sítě a koexistujících připojení](#new).
* Už mám virtuální síť modelu nasazení Resource Manager.
  
    Už můžete mít virtuální síť s existujícím připojením VPN typu site-to-site nebo připojením ExpressRoute. Hello [tooconfigure koexistujících připojení pro už existující virtuální síť](#add) části najdete postup odstranění hello brány a následného vytvoření nových připojení ExpressRoute a Site-to-Site VPN. Při vytváření nové připojení hello hello kroky musí dokončit v určitém pořadí. Nepoužívejte hello pokyny v jiných článcích toocreate připojení a bran.
  
    V tomto postupu vytvoření připojení, která mohou existovat vedle sebe vyžaduje toodelete můžete bránu a pak nakonfigurovali nové brány. Budete mít výpadku pro připojení mezi různými místy odstranit a znovu vytvořte brány a připojení, ale nebude nutné toomigrate všechny virtuální počítače a služby tooa nové virtuální sítě. Virtuální počítače a služby bude nadále možné toocommunicate se prostřednictvím nástroje pro vyrovnávání zatížení hello během konfigurace brány, pokud jsou nakonfigurované toodo tak.

## <a name="new"></a>toocreate nové virtuální sítě a koexistujících připojení
Tento postup vás provede procesem vytvoření virtuální sítě a připojení ExpressRoute a VPN typu Site-to-Site, která budou existovat společně.

1. Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell hello hello. Informace o instalaci rutin hello najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview). Hello rutin, které používáte pro tuto konfiguraci můžou mírně lišit, než co je znají. Být jisti toouse hello rutiny určené v těchto pokynech.
2. Přihlaste se v účtu tooyour a nastavení prostředí hello.

  ```powershell
  login-AzureRmAccount
  Select-AzureRmSubscription -SubscriptionName 'yoursubscription'
  $location = "Central US"
  $resgrp = New-AzureRmResourceGroup -Name "ErVpnCoex" -Location $location
  $VNetASN = 65010
  ```
3. Vytvořte virtuální síť včetně podsítě brány. Další informace o konfiguraci virtuální sítě hello najdete v tématu [konfigurace Azure Virtual Network](../virtual-network/virtual-networks-create-vnet-arm-ps.md).
   
   > [!IMPORTANT]
   > Hello podsíť brány musí být/27 nebo kratší předpona (například/26 nebo /25).
   > 
   > 
   
    Vytvořte novou virtuální síť.

  ```powershell
  $vnet = New-AzureRmVirtualNetwork -Name "CoexVnet" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AddressPrefix "10.200.0.0/16"
  ```
   
    Přidejte podsítě.

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name "App" -VirtualNetwork $vnet -AddressPrefix "10.200.1.0/24"
  Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
  ```
   
    Uložte konfiguraci virtuální sítě hello.

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
4. <a name="gw"></a>Vytvořte bránu ExpressRoute. Další informace o konfiguraci brány ExpressRoute hello najdete v tématu [konfigurace brány ExpressRoute](expressroute-howto-add-gateway-resource-manager.md). Hello GatewaySKU musí být *standardní*, *HighPerformance*, nebo *UltraPerformance*.

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "ERGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "ERGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  $gw = New-AzureRmVirtualNetworkGateway -Name "ERGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "ExpressRoute" -GatewaySku Standard
  ```
5. Propojení okruhu ExpressRoute toohello brány ExpressRoute hello. Po dokončení tohoto kroku se hello připojení mezi místní sítí a Azure prostřednictvím ExpressRoute vytvořeno. Další informace o operaci propojení hello najdete v tématu [propojení virtuálních sítí tooExpressRoute](expressroute-howto-linkvnet-arm.md).

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "YourCircuit" -ResourceGroupName "YourCircuitResourceGroup"
  New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $gw -PeerId $ckt.Id -ConnectionType ExpressRoute
  ```
6. <a name="vpngw"></a>Dále vytvořte bránu VPN typu site-to-site. Další informace o konfiguraci brány VPN hello najdete v tématu [konfigurace virtuální sítě pomocí připojení Site-to-Site](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md). Hello GatewaySKU musí být *standardní*, *HighPerformance*, nebo *UltraPerformance*. Hello VpnType musí *RouteBased*.

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "VPNGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "VPNGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard"
  ```
   
    Brána Azure VPN podporuje směrovací protokol BGP. Přidáte přepínač - Asn hello hello následující příkaz, můžete zadat číslo ASN (čísla AS) pro tuto virtuální síť. Není zadáním tohoto parametru bude tooAS výchozí číslo 65515.

  ```powershell
  $azureVpn = New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard" -Asn $VNetASN
  ```
   
    Můžete najít hello IP partnerského vztahu protokolu BGP a hello jako číslo, které Azure používá pro bránu VPN hello v $azureVpn.BgpSettings.BgpPeeringAddress a $azureVpn.BgpSettings.Asn. Další informace najdete v tématu [Konfigurace protokolu BGP](../vpn-gateway/vpn-gateway-bgp-resource-manager-ps.md) pro bránu VPN Azure.
7. Vytvořte entitu brány VPN místního webu. Tento příkaz neprovede konfiguraci vaší místní brány VPN. Místo toho můžete nastavení místní brány hello tooprovide, jako je například veřejná IP adresa hello a hello místní adresní prostor, aby hello Azure VPN gateway bude moct připojit tooit.
   
    Pokud vaše místní zařízení VPN podporuje jenom statické směrování, můžete nakonfigurovat statické trasy hello v hello následujícím způsobem:

  ```powershell
  $MyLocalNetworkAddress = @("10.100.0.0/16","10.101.0.0/16","10.102.0.0/16")
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress *<Public IP>* -AddressPrefix $MyLocalNetworkAddress
  ```
   
    Pokud vaše místní zařízení VPN podporuje hello protokolu BGP a chcete tooenable dynamické směrování, je třeba tooknow hello BGP partnerský vztah hello jako číslo, které vaše místní síť VPN a IP adresy zařízení používá.

  ```powershell
  $localVPNPublicIP = "<Public IP>"
  $localBGPPeeringIP = "<Private IP for hello BGP session>"
  $localBGPASN = "<ASN>"
  $localAddressPrefix = $localBGPPeeringIP + "/32"
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress $localVPNPublicIP -AddressPrefix $localAddressPrefix -BgpPeeringAddress $localBGPPeeringIP -Asn $localBGPASN
  ```
8. Nakonfigurujte místní zařízení tooconnect toohello nové Azure VPN bránu VPN. Další informace o konfiguraci zařízení VPN najdete v tématu [Konfigurace zařízení VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md).
9. Brána Site-to-Site VPN hello odkaz na Azure toohello místní brány.

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  New-AzureRmVirtualNetworkGatewayConnection -Name "VPNConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $azureVpn -LocalNetworkGateway2 $localVpn -ConnectionType IPsec -SharedKey <yourkey>
  ```

## <a name="add"></a>tooconfigure koexistujících připojení pro už existující virtuální síť
Pokud máte existující virtuální síť, zkontrolujte velikost podsítě brány hello. Pokud podsíť brány hello velikosti/28 nebo/29, musíte nejprve odstranit bránu virtuální sítě hello a zvýšit velikost podsítě brány hello. Hello kroky v této části ukazují, jak toodo který.

Pokud hello podsíť brány je/27 nebo větší a hello virtuální síť je připojená přes ExpressRoute, můžete přeskočit hello kroky a přejít příliš["Krok 6 – Vytvoření brány Site-to-Site VPN"](#vpngw) v předchozí části hello. 

> [!NOTE]
> Pokud odstraníte existující bránu hello, místní místo ztratí hello připojení tooyour virtuální sítě během práce na této konfiguraci. 
> 
> 

1. Budete potřebovat tooinstall hello nejnovější verzi rutin prostředí Azure PowerShell hello. Další informace o instalaci rutin najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview). Hello rutin, které používáte pro tuto konfiguraci můžou mírně lišit, než co je znají. Být jisti toouse hello rutiny určené v těchto pokynech. 
2. Odstraňte existující bránu ExpressRoute nebo VPN typu Site-to-Site na hello.

  ```powershell 
  Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
  ```
3. Odstraňte podsíť brány.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup> Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
  ```
4. Přidejte podsíť brány, která je /27 nebo větší.
   
   > [!NOTE]
   > Pokud nemáte dost IP adres v velikost podsítě brány vaší virtuální sítě tooincrease hello, musíte tooadd další adresní prostor IP adres.
   > 
   > 

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
  Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
  ```
   
    Uložte konfiguraci virtuální sítě hello.

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
5. V tuto chvíli máte virtuální síť, která nemá žádné brány. toocreate nové brány a dokončili připojení, abyste mohli pokračovat [krokem 4 – vytvoření brány ExpressRoute](#gw), který se nachází v hello předcházející sadě kroků.

## <a name="tooadd-point-to-site-configuration-toohello-vpn-gateway"></a>Brána sítě VPN toohello tooadd konfigurace point-to-site
Můžete provést kroky hello níže tooadd Point-to-Site konfigurace tooyour brány VPN v nastavení koexistence.

1. Přidejte fond adres klienta VPN.

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $azureVpn -VpnClientAddressPool "10.251.251.0/24"
  ```
2. Nahrajte hello VPN kořenový certifikát tooAzure pro bránu sítě VPN. V tomto příkladu se předpokládá, že tento hello kořenový certifikát je uložený v hello místního počítače, kde se spouštějí hello následující rutiny prostředí PowerShell.

  ```powershell
  $p2sCertFullName = "RootErVpnCoexP2S.cer" 
  $p2sCertMatchName = "RootErVpnCoexP2S" 
  $p2sCertToUpload=get-childitem Cert:\CurrentUser\My | Where-Object {$_.Subject -match $p2sCertMatchName} 
  if ($p2sCertToUpload.count -eq 1){write-host "cert found"} else {write-host "cert not found" exit} 
  $p2sCertData = [System.Convert]::ToBase64String($p2sCertToUpload.RawData) Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $p2sCertFullName -VirtualNetworkGatewayname $azureVpn.Name -ResourceGroupName $resgrp.ResourceGroupName -PublicCertData $p2sCertData
  ```

Další informace o VPN typu point-to-site najdete v tématu [Konfigurace připojení typu point-to-site](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md).

## <a name="next-steps"></a>Další kroky
Další informace o ExpressRoute najdete v tématu hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).
