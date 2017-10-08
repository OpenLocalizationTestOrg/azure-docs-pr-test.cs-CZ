---
title: "Nakonfigurujte vynucené tunelování pro připojení Azure Site-to-Site: classic | Microsoft Docs"
description: "Jak tooredirect nebo \"Vynutit\" všechny internetový provoz back tooyour místní umístění."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 5c0177f1-540c-4474-9b80-f541fa44240b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: 35b3a9ea370f9f962572629a69cc9aed16a87837
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-forced-tunneling-using-hello-classic-deployment-model"></a>Nakonfigurujte vynucené tunelování, pomocí modelu nasazení classic hello

Vynutit tunelové propojení umožňuje přesměrování nebo "Vynutit" všechny internetový provoz back tooyour místní umístění prostřednictvím tunelu Site-to-Site VPN pro kontrolu a auditování. Požadavek kritické zabezpečení pro většinu organizace IT zásad. Bez vynucené tunelování, internetový provoz z virtuálních počítačů v Azure bude vždy procházení od Azure síťové infrastruktury přímo na Internetu, toohello bez tooallow možnost hello je provoz hello tooinspect nebo kontrola. Neoprávněný přístup k Internetu může potenciálně vést tooinformation zpřístupnění nebo jiné typy narušení zabezpečení.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Tento článek vás provede procesem konfigurace vynucené tunelování pro virtuální sítě vytvořené pomocí modelu nasazení classic hello. Vynucené tunelování se dá konfigurovat pomocí prostředí PowerShell, ne přes portál hello. Pokud chcete tooconfigure vynuceného tunelování pro model nasazení Resource Manager hello, vyberte classic článek z hello následující rozevíracího seznamu:

> [!div class="op_single_selector"]
> * [PowerShell – Classic](vpn-gateway-about-forced-tunneling.md)
> * [PowerShell – Resource Manager](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="requirements-and-considerations"></a>Požadavky a důležité informace
Vynucené tunelování v Azure je nakonfigurován pomocí virtuální síti trasy definované uživatelem (UDR). Přesměrování provozu tooan místní lokality je vyjádřeno jako brána Azure VPN toohello výchozí trasu. Hello následující části jsou uvedeny hello aktuální omezení trasy a hello směrovací tabulky pro virtuální síť Azure:

* Každá podsíť virtuální sítě má integrovanou, směrovací tabulky systému. Hello systémovou tabulku směrování má hello následující tři skupiny tras:

  * **Místní virtuální síť trasy:** přímo toohello cílové virtuální počítače v hello stejné virtuální síti.
  * **Místní trasy:** toohello Azure VPN gateway.
  * **Výchozí trasu:** přímo toohello Internetu. Pakety určené toohello soukromé IP adresy není pokrytá hello předchozí dva trasy se zahodí.
* S vydáním hello trasy definované uživatelem můžete vytvořit směrovací tabulku tooadd výchozí trasu a pak přidružit hello směrovací tabulky tooyour virtuální síť podsítí tooenable vynucené tunelování na těchto podsítí.
* Je nutné tooset "výchozí web" mezi hello mezi různými místy místní lokality připojené toohello virtuální sítě.
* Vynucené tunelování musí být přidruženy k virtuální síti, která má dynamické směrování brána sítě VPN (není statická brána).
* ExpressRoute vynuceného tunelování přes tento mechanismus není nakonfigurovaná, ale místo toho je ve inzeruje výchozí trasu přes hello ExpressRoute BGP povolený pro relace partnerského vztahu. Najdete v tématu hello [dokumentace ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) Další informace.

## <a name="configuration-overview"></a>Přehled konfigurace
V následujícím příkladu hello tunelovým propojením hello front-endu podsíť není vynutit. Hello úlohy v podsíť Frontend hello můžete i nadále tooaccept a reagovat toocustomer požadavky z hello Internet přímo. Hello střední vrstvě a back-end podsítě, vynuceně přesunuty tunelového propojení. Všechny odchozí připojení z těchto dvou podsítí toohello Internetu bude tooan vynucené nebo přesměrované zpět na místní lokalitu prostřednictvím jednoho hello tunelovými propojeními S2S VPN.

To vám umožní toorestrict a kontrolovat přístup k Internetu z virtuálních počítačů nebo cloudových služeb v Azure, a přitom dál tooenable vaší architektury víceúrovňová služba vyžaduje. Také můžete použít vynucené tunelování toohello celý virtuální sítě Pokud nejsou žádná internetového zatížení ve virtuálních sítích.

![Vynucené tunelování](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)

## <a name="before-you-begin"></a>Než začnete
Ověřte, zda máte následující položky před počáteční konfigurace hello.

* Předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).
* Nakonfigurované virtuální sítě. 
* Hello nejnovější verze hello rutin prostředí Azure PowerShell. V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) Další informace o instalaci rutin prostředí PowerShell hello.

## <a name="configure-forced-tunneling"></a>Konfigurace vynuceného tunelování
Hello následující postup vám pomůže určit vynucené tunelování pro virtuální síť. Postup konfigurace Hello odpovídají toohello virtuální síť sítě konfigurační soubor.

```
<VirtualNetworkSite name="MultiTier-VNet" Location="North Europe">
     <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="DefaultSiteHQ">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch1">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch2">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch3">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
        </Gateway>
      </VirtualNetworkSite>
    </VirtualNetworkSite>
```

V tomto příkladu hello virtuální sítě "MultiTier-VNet, má tři podsítě: 'Front-endu', 'Midtier' a 'Back-end' podsítě s připojeními čtyři mezi více místy: 'DefaultSiteHQ' a tři větve. 

kroky Hello se nastavit hello 'DefaultSiteHQ' jako hello výchozí lokality připojení pro vynucené tunelování a nakonfigurujte hello Midtier a back-end podsítě toouse vynuceného tunelování.

1. Umožňuje vytvořte směrovací tabulku. Pomocí následující rutiny toocreate hello směrovací tabulku.

  ```powershell
  New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"
  ```
2. Přidejte výchozí směrovací toohello směrovací tabulku. 

  Hello následující příklad přidá výchozí trasy toohello směrovací tabulku vytvořili v kroku 1. Všimněte si, že hello pouze trasy podporována je předpona cílové hello "0.0.0.0/0" toohello "Brána VPN" dalšího segmentu.

  ```powershell
  Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway
  ```
3. Přidružte hello směrovací tabulky toohello podsítě. 

  Po vytvoření směrovací tabulku a přidat trasu, použijte následující příklad tooadd hello nebo přidružit hello trasy tabulky tooa VNet podsíť. Příklad Hello přidá hello trasy tabulky "MyRouteTable" toohello Midtier a back-end podsítě virtuální sítě MultiTier-VNet.

  ```powershell
  Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"
  Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"
  ```
4. Přiřadíte výchozí web pro vynucené tunelování. 

  V předchozím kroku hello hello ukázkové rutiny skripty vytvořili hello směrovací tabulky a související hello trasy tabulky tootwo hello podsítí virtuální sítě. zbývající krok Hello je tooselect místní sítě mezi připojení více lokalit hello hello jako výchozí web hello nebo tunelové propojení virtuální sítě.

  ```powershell
  $DefaultSite = @("DefaultSiteHQ")
  Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"
  ```

## <a name="additional-powershell-cmdlets"></a>Další rutiny prostředí PowerShell
### <a name="toodelete-a-route-table"></a>toodelete směrovací tabulku

```powershell
Remove-AzureRouteTable -Name <routeTableName>
```
  
### <a name="toolist-a-route-table"></a>toolist směrovací tabulku

```powershell
Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]
```

### <a name="toodelete-a-route-from-a-route-table"></a>toodelete trasy z tabulky trasy

```powershell
Remove-AzureRouteTable –Name <routeTableName>
```

### <a name="tooremove-a-route-from-a-subnet"></a>tooremove trasy z podsítě

```powershell
Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="toolist-hello-route-table-associated-with-a-subnet"></a>Tabulka směrování hello toolist spojené s podsítí

```powershell
Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="tooremove-a-default-site-from-a-vnet-vpn-gateway"></a>tooremove výchozí web z brány virtuální sítě VPN

```powershell
Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>
```