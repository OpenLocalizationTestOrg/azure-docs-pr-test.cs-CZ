---
title: "Nakonfigurujte vynucené tunelování pro připojení Azure Site-to-Site: Resource Manager | Microsoft Docs"
description: "Jak tooredirect nebo \"Vynutit\" všechny internetový provoz back tooyour místní umístění."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: cbe58db8-b598-4c9f-ac88-62c865eb8721
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: cherylmc
ms.openlocfilehash: 6bc52c04ab0749a674c9863be5e4f9a9f7c98df4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-forced-tunneling-using-hello-azure-resource-manager-deployment-model"></a>Nakonfigurujte vynucené tunelování, pomocí modelu nasazení Azure Resource Manager hello

Vynutit tunelové propojení umožňuje přesměrování nebo "Vynutit" všechny internetový provoz back tooyour místní umístění prostřednictvím tunelu Site-to-Site VPN pro kontrolu a auditování. Požadavek kritické zabezpečení pro většinu organizace IT zásad. Bez vynucené tunelování, internetový provoz z virtuálních počítačů v Azure vždy prochází od Azure síťové infrastruktury přímo na Internetu, toohello bez tooallow možnost hello je provoz hello tooinspect nebo kontrola. Neoprávněný přístup k Internetu může potenciálně vést tooinformation zpřístupnění nebo jiné typy narušení zabezpečení.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Tento článek vás provede procesem konfigurace vynucené tunelování pro virtuální sítě vytvořené pomocí modelu nasazení Resource Manager hello. Vynucené tunelování se dá konfigurovat pomocí prostředí PowerShell, ne přes portál hello. Pokud chcete tooconfigure vynuceného tunelování pro model nasazení classic hello, vyberte classic článek z hello následující rozevíracího seznamu:

> [!div class="op_single_selector"]
> * [PowerShell – Classic](vpn-gateway-about-forced-tunneling.md)
> * [PowerShell – Resource Manager](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="about-forced-tunneling"></a>O vynuceného tunelování

Hello následující diagram znázorňuje, jak vynucené tunelování funguje. 

![Vynucené tunelování](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

V příkladu hello výše hello tunelovým propojením front-endu podsíť není vynutit. Hello úlohy v podsíť Frontend hello můžete i nadále tooaccept a reagovat toocustomer požadavky z hello Internet přímo. Hello střední vrstvě a back-end podsítě, vynuceně přesunuty tunelového propojení. Všechny odchozí připojení z těchto dvou podsítí toohello Internetu bude tooan vynucené nebo přesměrované zpět na místní lokalitu prostřednictvím jednoho hello tunelovými propojeními S2S VPN.

To vám umožní toorestrict a kontrolovat přístup k Internetu z virtuálních počítačů nebo cloudových služeb v Azure, a přitom dál tooenable vaší architektury víceúrovňová služba vyžaduje. Pokud nejsou žádná internetového zatížení ve virtuálních sítích, můžete také použít vynucené tunelování toohello celý virtuální sítě.

## <a name="requirements-and-considerations"></a>Požadavky a důležité informace

Vynucené tunelování v Azure je nakonfigurován pomocí virtuální síti trasy definované uživatelem. Přesměrování provozu tooan místní lokality je vyjádřeno jako brána Azure VPN toohello výchozí trasu. Další informace o směrování definovaného uživatelem a virtuální sítě najdete v tématu [trasy definované uživatelem a předávání IP](../virtual-network/virtual-networks-udr-overview.md).

* Každá podsíť virtuální sítě má integrovanou, směrovací tabulky systému. Hello systémovou tabulku směrování má hello následující tři skupiny tras:
  
  * **Místní virtuální síť trasy:** přímo toohello cílové virtuální počítače v hello stejné virtuální síti.
  * **Místní trasy:** toohello Azure VPN gateway.
  * **Výchozí trasu:** přímo toohello Internetu. Jsou pakety určené toohello soukromé IP adresy není pokrytá hello předchozí dva trasy vyřadit.
* Tento postup používá trasy definované uživatelem (UDR) toocreate směrovací tabulky tooadd výchozí trasu a pak přidružit hello směrování tabulky tooyour VNet podsítí tooenable vynucené tunelování na těchto podsítí.
* Vynucené tunelování musí být přidružený virtuální síť, která má brána sítě VPN založené na trasách. Je nutné tooset "výchozí web" mezi hello mezi různými místy místní lokality připojené toohello virtuální sítě. Navíc hello místního zařízení VPN musí být nakonfigurovaný pomocí 0.0.0.0/0 jako provoz selektorů. 
* ExpressRoute vynuceného tunelování přes tento mechanismus není nakonfigurovaná, ale místo toho je ve inzeruje výchozí trasu přes hello ExpressRoute BGP povolený pro relace partnerského vztahu. Další informace najdete v tématu hello [dokumentace ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/).

## <a name="configuration-overview"></a>Přehled konfigurace

Hello následující postup vám pomůže vytvořit skupinu prostředků a virtuální sítě. Potom můžete vytvořit bránu sítě VPN a nakonfigurujte vynucené tunelování. V tomto postupu hello virtuální sítě "MultiTier-VNet, má tři podsítě:"Frontend","Midtier"a"Backend", s čtyři mezi různými místy připojení: 'DefaultSiteHQ' a tři větve.

kroky postupu Hello nastavit hello 'DefaultSiteHQ', protože připojení site hello výchozí pro vynuceného tunelování a konfiguraci hello 'Midtier' a 'Back-end' podsítě toouse vynuceného tunelování.

## <a name="before-you-begin"></a>Než začnete

Nainstalujte nejnovější verzi rutin Powershellu pro Azure Resource Manager hello hello. V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) Další informace o instalaci rutin prostředí PowerShell hello.

### <a name="toolog-in"></a>toolog v

[!INCLUDE [toolog in](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="configure-forced-tunneling"></a>Konfigurace vynuceného tunelování

1. Vytvořte skupinu prostředků.

  ```powershell
  New-AzureRmResourceGroup -Name 'ForcedTunneling' -Location 'North Europe'
  ```
2. Vytvoření virtuální sítě a určit podsítě.

  ```powershell 
  $s1 = New-AzureRmVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
  $s2 = New-AzureRmVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
  $s3 = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
  $s4 = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
  $vnet = New-AzureRmVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4
  ```
3. Vytvořte hello brány místní sítě.

  ```powershell
  $lng1 = New-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
  $lng2 = New-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
  $lng3 = New-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
  $lng4 = New-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
  ```
4. Vytvořte pravidlo tabulky a směrování trasy hello.

  ```powershell
  New-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
  $rt = Get-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
  Add-AzureRmRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
  Set-AzureRmRouteTable -RouteTable $rt
  ```
5. Přidružte hello trasy tabulky toohello Midtier a back-end podsítě.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
  Set-AzureRmVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
  Set-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
6. Vytvořte hello brány s výchozí web. Tento krok trvá některé toocomplete čas, někdy 45 minut nebo déle, protože jsou vytvoření a konfiguraci brány hello.<br> Hello **- GatewayDefaultSite** je hello parametr rutiny, která umožňuje hello vynutit směrování toowork konfigurace, takže trvat pozor tooconfigure toto nastavení správně. Tento parametr je k dispozici v prostředí PowerShell 1.0 nebo novější.

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
  $gwsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $ipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
  New-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -GatewayDefaultSite $lng1 -EnableBgp $false
  ```
7. Navázání připojení VPN typu Site-to-Site hello.

  ```powershell
  $gateway = Get-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
  $lng1 = Get-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" 
  $lng2 = Get-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" 
  $lng3 = Get-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" 
  $lng4 = Get-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" 
    
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng1 -ConnectionType IPsec -SharedKey "preSharedKey"
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng2 -ConnectionType IPsec -SharedKey "preSharedKey"
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng3 -ConnectionType IPsec -SharedKey "preSharedKey"
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection4" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng4 -ConnectionType IPsec -SharedKey "preSharedKey"
    
  Get-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling"
  ```