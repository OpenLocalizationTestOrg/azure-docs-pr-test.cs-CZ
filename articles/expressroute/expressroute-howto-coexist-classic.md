---
title: "Konfigurace ExpressRoute a připojení VPN typu site-to-site, která mohou existovat vedle sebe: Classic: Azure | Dokumentace Microsoftu"
description: "Tento článek vás provede konfigurací ExpressRoute a připojení Site-to-Site VPN, která mohou existovat vedle sebe pro model nasazení classic hello."
documentationcenter: na
services: expressroute
author: charwen
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: dcf1a5af-a289-466a-b812-0bfedbd2bda0
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: charwen
ms.openlocfilehash: abb30fff55e8ec243f2920c5b2f70c43717755fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-expressroute-and-site-to-site-coexisting-connections-classic"></a>Konfigurace společně používaných připojení typu Site-to-Site a ExpressRoute (Classic)
> [!div class="op_single_selector"]
> * [PowerShell – Resource Manager](expressroute-howto-coexist-resource-manager.md)
> * [PowerShell – Classic](expressroute-howto-coexist-classic.md)
> 
> 

Hello možnost tooconfigure VPN Site-to-Site a ExpressRoute má několik výhod. Můžete nakonfigurovat VPN typu Site-to-Site jako cestu zabezpečené převzetí služeb při selhání pro ExressRoute, nebo použít toosites tooconnect sítě Site-to-Site VPN, které nejsou připojené prostřednictvím ExpressRoute. Vám nabídneme hello kroky tooconfigure oba scénáře v tomto článku. Tento článek se týká modelu nasazení classic toohello. Tato konfigurace není v portálu hello k dispozici.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**O modelech nasazení Azure**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

> [!IMPORTANT]
> Okruhy ExpressRoute musí být předem nakonfigurované, než začnete postupovat podle pokynů hello. Ujistěte se, že jste postupovali podle příručky hello příliš[vytvoření okruhu ExpressRoute](expressroute-howto-circuit-classic.md) a [konfigurace směrování](expressroute-howto-routing-classic.md) před provedením následujících kroků hello.
> 
> 

## <a name="limits-and-limitations"></a>Omezení
* **Směrování provozu není podporováno.** Nemůžete provádět směrování (přes Azure) mezi místní sítí připojenou prostřednictvím sítě VPN typu site-to-site a místní sítí připojenou přes ExpressRoute.
* **Typ point-to-site není podporován.** Nelze povolit toohello připojení point-to-site VPN stejnou virtuální síť, která je připojená tooExpressRoute. Síť VPN Point-to-site a ExpressRoute nemůžou existovat pro hello stejné virtuální síti.
* **Na hello brány VPN Site-to-Site nejde povolit vynucené tunelování.** Můžete jenom "Vynutit" všechny internetový provoz back tooyour místní sítě prostřednictvím ExpressRoute.
* **Základní brána SKU není podporována.** Musíte použít bránu bez – základní SKU pro obě hello [brány ExpressRoute](expressroute-about-virtual-network-gateways.md) a hello [brány VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md).
* **Podporována je pouze brána VPN na základě tras.** Je nutné použít službu [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) na základě tras.
* **Pro vaši bránu VPN by měla být nakonfigurována statická trasa.** Pokud vaše místní síť připojená tooboth ExpressRoute a Site-to-Site VPN, musíte mít ve vaší místní síti tooroute hello Site-to-Site VPN připojení toohello konfigurovanou statickou trasu veřejného Internetu.
* **Nejprve je nutné nakonfigurovat bránu ExpressRoute.** Předtím, než přidáte bránu VPN typu Site-to-Site hello musí nejprve vytvořte bránu ExpressRoute hello.

## <a name="configuration-designs"></a>Návrhy konfigurace
### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a>Konfigurace VPN typu site-to-site jako cesty převzetí služeb při selhání pro ExpressRoute
Můžete nakonfigurovat připojení VPN typu site-to-site jako záložní pro ExpressRoute. To platí pouze toovirtual sítě propojené toohello cestou soukromého partnerského vztahu Azure. Neexistuje žádné řešení převzetí služeb při selhání založené na VPN pro služby, které jsou přístupné prostřednictvím veřejného partnerského vztahu Azure nebo partnerského vztahu Microsoftu. Hello okruh ExpressRoute je vždy hello primární propojení. Data budou procházet prostřednictvím cesty hello Site-to-Site VPN, pouze pokud hello okruh ExpressRoute selže. 

> [!NOTE]
> Při okruh ExpressRoute je upřednostňované prostřednictvím sítě Site-to-Site VPN, když oba trasy jsou hello stejné, použije Azure hello nejdelší předponu shodu toochoose hello trasy směrem cíle hello paketu.
> 
> 

![Současná existence](media/expressroute-howto-coexist-classic/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-tooconnect-toosites-not-connected-through-expressroute"></a>Konfigurace Site-to-Site VPN tooconnect toosites nejsou připojené prostřednictvím ExpressRoute
Můžete nakonfigurovat síti, kde některé weby jsou připojené přímo tooAzure prostřednictvím sítě Site-to-Site VPN a některé weby přes ExpressRoute. 

![Současná existence](media/expressroute-howto-coexist-classic/scenario2.jpg)

> [!NOTE]
> Virtuální síť nemůžete nakonfigurovat jako směrovač provozu.
> 
> 

## <a name="selecting-hello-steps-toouse"></a>Výběr toouse kroky hello
Existují dvě sady postupů toochoose z v pořadí tooconfigure připojení, které mohou existovat vedle sebe. Postup konfigurace Hello, který vyberete, bude záviset na tom, jestli máte existující virtuální síť, které mají tooconnect k, nebo chcete toocreate nové virtuální sítě.

* Nemám virtuální síť a potřebuji toocreate jeden.
  
    Pokud ještě nemáte virtuální síť, tento postup vás provede vytvořením nové virtuální sítě pomocí modelu nasazení classic hello a vytvoření nových připojení ExpressRoute a Site-to-Site VPN. tooconfigure, postupujte podle kroků hello v části článku hello [toocreate nové virtuální sítě a koexistujících připojení](#new).
* Už mám virtuální síť modelu nasazení Classic.
  
    Už můžete mít virtuální síť s existujícím připojením VPN typu site-to-site nebo připojením ExpressRoute. Hello části [tooconfigure koexistujících připojení pro už existující virtuální síť](#add) vás provede procesem odstranění hello brány a následného vytvoření nových připojení ExpressRoute a VPN typu Site-to-Site. Všimněte si, že při vytváření hello nová připojení, musí hello kroky provedené ve velmi specifickém pořadí. Nepoužívejte hello pokyny v jiných článcích toocreate připojení a bran.
  
    V tomto postupu vytvoření připojení, která mohou existovat vedle sebe vyžadují toodelete můžete bránu a pak nakonfigurovali nové brány. To znamená, že budete mít výpadku pro připojení mezi různými místy odstranit a znovu vytvořte brány a připojení, ale nebude nutné toomigrate všechny virtuální počítače a služby tooa nové virtuální sítě. Virtuální počítače a služby bude nadále možné toocommunicate se prostřednictvím nástroje pro vyrovnávání zatížení hello během konfigurace brány, pokud jsou nakonfigurované toodo tak.

## <a name="new"></a>toocreate nové virtuální sítě a koexistujících připojení
Tento postup vás provede procesem vytvoření virtuální sítě a vytvoření připojení ExpressRoute a VPN site-to-site, která budou existovat společně.

1. Budete potřebovat tooinstall hello nejnovější verzi rutin prostředí Azure PowerShell hello. V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) Další informace o instalaci rutin prostředí PowerShell hello. Všimněte si, že může být hello rutin, které budete používat pro tuto konfiguraci poněkud liší od co je znají. Být jisti toouse hello rutiny určené v těchto pokynech. 
2. Vytvořte schéma pro virtuální síť. Další informace o schématu konfigurace hello najdete v tématu [schéma konfigurace Azure Virtual Network](https://msdn.microsoft.com/library/azure/jj157100.aspx).
   
    Při vytváření schématu, ujistěte se, že používáte hello následující hodnoty:
   
   * Hello podsíť brány pro virtuální síť hello musí být/27 nebo kratší předpona (například/26 nebo /25).
   * Typ připojení brány Hello je "Dedicated".
     
             <VirtualNetworkSite name="MyAzureVNET" Location="Central US">
               <AddressSpace>
                 <AddressPrefix>10.17.159.192/26</AddressPrefix>
               </AddressSpace>
               <Subnets>
                 <Subnet name="Subnet-1">
                   <AddressPrefix>10.17.159.192/27</AddressPrefix>
                 </Subnet>
                 <Subnet name="GatewaySubnet">
                   <AddressPrefix>10.17.159.224/27</AddressPrefix>
                 </Subnet>
               </Subnets>
               <Gateway>
                 <ConnectionsToLocalNetwork>
                   <LocalNetworkSiteRef name="MyLocalNetwork">
                     <Connection type="Dedicated" />
                   </LocalNetworkSiteRef>
                 </ConnectionsToLocalNetwork>
               </Gateway>
             </VirtualNetworkSite>
3. Po vytvoření a konfiguraci souboru schématu xml, nahrajte soubor hello. Tím vytvoříte virtuální síť.
   
    Pomocí následující rutiny tooupload hello souboru, nahraďte hello hodnoty vlastními.
   
        Set-AzureVNetConfig -ConfigurationPath 'C:\NetworkConfig.xml'
4. <a name="gw"></a>Vytvořte bránu ExpressRoute. Být jisti toospecify hello GatewaySKU jako *standardní*, *HighPerformance*, nebo *UltraPerformance* a hello GatewayType jako *DynamicRouting*.
   
    Použijte hello následující ukázka, nahraďte hello hodnoty vlastními.
   
        New-AzureVNetGateway -VNetName MyAzureVNET -GatewayType DynamicRouting -GatewaySKU HighPerformance
5. Propojení okruhu ExpressRoute toohello brány ExpressRoute hello. Po dokončení tohoto kroku se hello připojení mezi místní sítí a Azure prostřednictvím ExpressRoute vytvořeno.
   
        New-AzureDedicatedCircuitLink -ServiceKey <service-key> -VNetName MyAzureVNET
6. <a name="vpngw"></a>Dále vytvořte bránu VPN typu site-to-site. Hello GatewaySKU musí být *standardní*, *HighPerformance*, nebo *UltraPerformance* a hello GatewayType musí být *DynamicRouting*.
   
        New-AzureVirtualNetworkGateway -VNetName MyAzureVNET -GatewayName S2SVPN -GatewayType DynamicRouting -GatewaySKU  HighPerformance
   
    tooretrieve hello nastavení brány virtuální sítě, včetně ID brány hello a hello veřejné IP adresy, použijte hello `Get-AzureVirtualNetworkGateway` rutiny.
   
        Get-AzureVirtualNetworkGateway
   
        GatewayId            : 348ae011-ffa9-4add-b530-7cb30010565e
        GatewayName          : S2SVPN
        LastEventData        :
        GatewayType          : DynamicRouting
        LastEventTimeStamp   : 5/29/2015 4:41:41 PM
        LastEventMessage     : Successfully created a gateway for hello following virtual network: GNSDesMoines
        LastEventID          : 23002
        State                : Provisioned
        VIPAddress           : 104.43.x.y
        DefaultSite          :
        GatewaySKU           : HighPerformance
        Location             :
        VnetId               : 979aabcf-e47f-4136-ab9b-b4780c1e1bd5
        SubnetId             :
        EnableBgp            : False
        OperationDescription : Get-AzureVirtualNetworkGateway
        OperationId          : 42773656-85e1-a6b6-8705-35473f1e6f6a
        OperationStatus      : Succeeded
7. Vytvořte entitu brány VPN místního webu. Tento příkaz neprovede konfiguraci vaší místní brány VPN. Místo toho můžete nastavení místní brány hello tooprovide, jako je například veřejná IP adresa hello a hello místní adresní prostor, aby hello Azure VPN gateway bude moct připojit tooit.
   
   > [!IMPORTANT]
   > místní web Hello hello Site-to-Site VPN není definován v hello netcfg. Místo toho musíte použít tento parametry rutiny toospecify hello místního webu. Nelze definovat pomocí portálu nebo souboru netcfg hello.
   > 
   > 
   
    Použijte hello následující ukázka, nahraďte hello hodnoty vlastními.
   
        New-AzureLocalNetworkGateway -GatewayName MyLocalNetwork -IpAddress <MyLocalGatewayIp> -AddressSpace <MyLocalNetworkAddress>
   
   > [!NOTE]
   > Pokud vaše místní síť obsahuje víc tras, můžete je předat všechny najednou jako pole.  $MyLocalNetworkAddress = @("10.1.2.0/24","10.1.3.0/24","10.2.1.0/24")  
   > 
   > 

    tooretrieve hello nastavení brány virtuální sítě, včetně ID brány hello a hello veřejné IP adresy, použijte hello `Get-AzureVirtualNetworkGateway` rutiny. Viz následující ukázka hello.

        Get-AzureLocalNetworkGateway

        GatewayId            : 532cb428-8c8c-4596-9a4f-7ae3a9fcd01b
        GatewayName          : MyLocalNetwork
        IpAddress            : 23.39.x.y
        AddressSpace         : {10.1.2.0/24}
        OperationDescription : Get-AzureLocalNetworkGateway
        OperationId          : ddc4bfae-502c-adc7-bd7d-1efbc00b3fe5
        OperationStatus      : Succeeded


1. Nakonfigurujte místní zařízení tooconnect toohello novou bránu VPN. Použijte hello informace, které jste získali v kroku 6 při konfiguraci zařízení VPN. Další informace o konfiguraci zařízení VPN najdete v tématu [Konfigurace zařízení VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md).
2. Brána Site-to-Site VPN hello odkaz na Azure toohello místní brány.
   
    V tomto příkladu je connectedEntityId hello ID místní brány, které můžete najít spuštěním `Get-AzureLocalNetworkGateway`. VirtualNetworkGatewayId můžete najít pomocí hello `Get-AzureVirtualNetworkGateway` rutiny. Po provedení tohoto kroku hello připojení mezi místní sítí a Azure prostřednictvím hello připojení Site-to-Site VPN je vytvořeno.

        New-AzureVirtualNetworkGatewayConnection -connectedEntityId <local-network-gateway-id> -gatewayConnectionName Azure2Local -gatewayConnectionType IPsec -sharedKey abc123 -virtualNetworkGatewayId <azure-s2s-vpn-gateway-id>

## <a name="add"></a>tooconfigure koexistujících připojení pro už existující virtuální síť
Pokud máte existující virtuální síť, zkontrolujte velikost podsítě brány hello. Pokud podsíť brány hello velikosti/28 nebo/29, musíte nejprve odstranit bránu virtuální sítě hello a zvýšit velikost podsítě brány hello. Hello kroky v této části se dozvíte, jak toodo který.

Pokud hello podsíť brány je/27 nebo větší a hello virtuální síť je připojená přes ExpressRoute, můžete přeskočit hello kroky a přejít příliš["Krok 6 – Vytvoření brány Site-to-Site VPN"](#vpngw) v předchozí části hello.

> [!NOTE]
> Pokud odstraníte existující bránu hello, místní místo ztratí hello připojení tooyour virtuální sítě během práce na této konfiguraci.
> 
> 

1. Budete potřebovat tooinstall hello nejnovější verzi hello rutiny Powershellu pro Azure Resource Manager. V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) Další informace o instalaci rutin prostředí PowerShell hello. Všimněte si, že může být hello rutin, které budete používat pro tuto konfiguraci poněkud liší od co je znají. Být jisti toouse hello rutiny určené v těchto pokynech. 
2. Odstraňte existující bránu ExpressRoute nebo VPN typu Site-to-Site na hello. Použijte následující rutinu, nahraďte hello hodnoty vlastními hello.
   
        Remove-AzureVNetGateway –VnetName MyAzureVNET
3. Exportujte schéma virtuální sítě hello. Použijte hello následující rutiny prostředí PowerShell, nahraďte hello hodnoty vlastními.
   
        Get-AzureVNetConfig –ExportToFile “C:\NetworkConfig.xml”
4. Upravte schéma konfiguračního souboru hello sítě tak, aby hello podsíť brány je/27 nebo kratší předpona (například/26 nebo /25). Viz následující ukázka hello. 
   
   > [!NOTE]
   > Pokud nemáte dost IP adres v velikost podsítě brány vaší virtuální sítě tooincrease hello, musíte tooadd další adresní prostor IP adres. Další informace o schématu konfigurace hello najdete v tématu [schéma konfigurace Azure Virtual Network](https://msdn.microsoft.com/library/azure/jj157100.aspx).
   > 
   > 
   
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.17.159.224/27</AddressPrefix>
          </Subnet>
5. Pokud předchozí brána byla VPN typu Site-to-Site, musíte změnit taky typ připojení hello příliš**Dedicated**.
   
                 <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="MyLocalNetwork">
                      <Connection type="Dedicated" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
6. V tuto chvíli máte virtuální síť, která nemá žádné brány. toocreate nové brány a dokončili připojení, abyste mohli pokračovat [krokem 4 – vytvoření brány ExpressRoute](#gw), který se nachází v hello předcházející sadě kroků.

## <a name="next-steps"></a>Další kroky
Další informace o ExpressRoute najdete v tématu hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md)

