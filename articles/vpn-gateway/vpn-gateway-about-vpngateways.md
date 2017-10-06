---
title: "Přehled brány sítě VPN: Vytvořte připojení VPN mezi různými místy tooAzure virtuální sítě | Microsoft Docs"
description: "Tento přehled brány VPN vysvětluje hello způsoby tooconnect tooAzure virtuálních sítí pomocí připojení k síti VPN přes hello Internet. Součástí článku jsou diagramy základní konfigurace připojení."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 2358dd5a-cd76-42c3-baf3-2f35aadc64c8
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/05/2017
ms.author: cherylmc
ms.openlocfilehash: 899270734270632a5b12d56021c924e977725a7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="about-vpn-gateway"></a>Informace o službě VPN Gateway

Brána sítě VPN je typ brány virtuální sítě, který odesílá šifrovaný provoz mezi tooan veřejného připojení na místní umístění. Můžete také použít toosend šifrované přenosy bran VPN mezi virtuálními sítěmi Azure přes síť Microsoft hello. toosend šifrované síťový provoz mezi virtuální sítě Azure a místními servery, je nutné vytvořit bránu VPN pro virtuální síť.

Každá virtuální síť může mít pouze jednu bránu VPN, ale můžete vytvořit více připojení toohello stejnou bránou sítě VPN. Příkladem je konfigurace připojení typu Multi-Site. Když vytvoříte více připojení toohello stejné brány sítě VPN, všechna tunelové propojení sítí VPN Point-to-Site VPN, sdílená šířka pásma hello, která je k dispozici pro bránu hello.

### <a name="whatis"></a>Co je brána virtuální sítě?

Brána virtuální sítě se skládá ze dvou nebo více virtuálních počítačů, které jsou nasazené tooa konkrétní podsíť s názvem hello GatewaySubnet. Hello virtuálních počítačů, které se nacházejí v hello GatewaySubnet se vytvoří při vytvoření brány virtuální sítě hello. Brána virtuální sítě virtuálních počítačů jsou nakonfigurované toocontain směrovacích tabulek a brány konkrétní toohello služby brány. Nelze konfigurovat přímo hello virtuálních počítačů, které jsou součástí brány virtuální sítě hello a měli byste nikdy nasadit další prostředky toohello GatewaySubnet.

Když vytvoříte bránu virtuální sítě pomocí typ brány hello 'Vpn', vytvoří určitý typ brány virtuální sítě, který šifruje provoz; Brána sítě VPN. Brána sítě VPN může trvat až toocreate too45 minut. To je kvůli hello virtuálních počítačů pro bránu VPN hello jsou nasazené toohello GatewaySubnet a nakonfigurované hello nastavení, které jste zadali. Hello skladová položka brány, kterou jste vybrali Určuje, jak výkonné hello virtuální počítače.

## <a name="gwsku"></a>SKU brány

[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

## <a name="configuring"></a>Konfigurace služby VPN Gateway

Připojení brány VPN se spoléhá na několik prostředků nakonfigurovaných se specifickými nastaveními. Většinu hello prostředků můžete nakonfigurovat samostatně, přestože musí být nakonfigurovány v určitém pořadí v některých případech.

### <a name="settings"></a>Nastavení

Hello nastavení, které jste zvolili pro jednotlivé zdroje jsou kritické toocreating úspěšné připojení. Informace o jednotlivých prostředcích a nastaveních služby VPN Gateway najdete v tématu [Informace o nastavení služby VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md). Hello článek obsahuje informace o toohelp porozumíte brány typy, typy sítě VPN, typy připojení, podsítě brány, brány místní sítě a různá nastavení prostředků, může být vhodné tooconsider.

### <a name="tools"></a>Nástroje pro nasazení

Můžete začít vytváření a konfigurace prostředků pomocí nástroj konfigurace, jako je například hello portálu Azure. Později můžete rozhodnout tooswitch tooanother nástroji, jako prostředí PowerShell, tooconfigure další prostředky, nebo upravit existující prostředky v případě potřeby. V současné době nelze konfigurovat každý prostředek a nastavení prostředků na hello portálu Azure. Při nástroje konkrétní konfigurace je potřeba zadat Hello pokyny v hello články pro každou topologie připojení. 

### <a name="models"></a>Model nasazení

Při konfiguraci brány VPN hello kroky, které můžete provést závisí na modelu nasazení hello používá toocreate vaší virtuální sítě. Například pokud jste vytvořili virtuální sítě pomocí modelu nasazení classic hello, použijte hello pokyny a pokyny pro nasazení classic hello modelu toocreate a nakonfigurovat nastavení brány sítě VPN. Další informace o modelech nasazení najdete v tématu [Pochopení modelů nasazení Resource Manager a Classic](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="diagrams"></a>Diagramy topologie připojení

Je důležité tooknow, že jsou k dispozici pro připojení brány VPN různých konfigurací. Je nutné toodetermine konfiguraci, kterou nejlepší vyhovuje vašim potřebám. V následujících částech hello, můžete zobrazit diagramy topologie a informace o hello následující brány sítě VPN: hello následující části obsahují tabulky, které seznamu:

* dostupný model nasazení,
* dostupné konfigurační nástroje,
* Odkazy, které vás zavedou přímo tooan článek, pokud je k dispozici

Použití hello diagramy a popisy toohelp vyberte toomatch topologie připojení hello vašim požadavkům. Hello diagramech hello základní topologie, ale je možné toobuild složitějších konfigurací pomocí hello diagramů jako vodítek.

## <a name="s2smulti"></a>Site-to-Site a Multi-Site (tunel VPN IPsec/IKE)

### <a name="S2S"></a>Site-to-Site

Připojení brány VPN typu Site-to-Site (S2S) je připojení přes tunel VPN prostřednictvím protokolu IPsec/IKE (IKEv1 nebo IKEv2). Připojení S2S vyžaduje sítě VPN zařízení nachází v místě které má veřejnou IP adresu přiřazenou tooit a není umístěný za adres (NAT) Připojení S2S můžete použít pro konfigurace mezi různými místy a pro hybridní konfigurace.   

![Příklad propojení Site-to-Site pomocí Azure VPN Gateway](./media/vpn-gateway-about-vpngateways/vpngateway-site-to-site-connection-diagram.png)

### <a name="Multi"></a>Multi-Site

Tento typ připojení je varianta hello připojení Site-to-Site. Vytvořit více než jedno připojení VPN z brány virtuální sítě, obvykle připojení toomultiple místními lokalitami. Při práci s několika připojeními je nutné použít typ sítě VPN RouteBased (při práci s klasickými virtuálními sítěmi se označuje jako dynamická brána). Protože každá virtuální síť může mít pouze jednu bránu VPN, všechna připojení prostřednictvím brány hello sdílet hello dostupnou šířku pásma. To se často označuje jako připojení „více lokalit“ (Multi-Site).

![Příklad propojení Multi-Site pomocí Azure VPN Gateway](./media/vpn-gateway-about-vpngateways/vpngateway-multisite-connection-diagram.png)

### <a name="deployment-models-and-methods-for-site-to-site-and-multi-site"></a>Modely nasazení a metody pro Site-to-Site a Multi-Site

[!INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)]

## <a name="P2S"></a>Point-to-Site (VPN prostřednictvím protokolu SSTP)

Brána sítě VPN typu Point-to-Site (P2S) umožňuje vytvoření bezpečného připojení virtuální sítě tooyour z jednotlivých klientských počítačů. Připojení point-to-Site VPN jsou užitečné, když chcete, aby tooconnect tooyour virtuální síti ze vzdáleného umístění, například při jsou telefonicky z domova nebo z konference. Síť VPN P2S je také užitečné řešení toouse místo Site-to-Site VPN, pokud máte pouze několik klientů, kteří potřebují tooconnect tooa virtuální sítě. 

Na rozdíl od připojení S2S nevyžadují připojení P2S místní veřejnou IP adresu ani zařízení VPN. Připojení P2S lze použít s připojení S2S prostřednictvím hello stejné brány sítě VPN, tak dlouho, dokud všechny hello požadavky na konfiguraci pro obě připojení jsou kompatibilní.

Používá P2S hello Secure Socket SSTP (Tunneling Protocol), což je protokol VPN založené na protokolu SSL. Připojení P2S VPN je vytvořeno spuštěním z hello klientského počítače.

![Příklad propojení Point-to-Site pomocí Azure VPN Gateway](./media/vpn-gateway-about-vpngateways/vpngateway-point-to-site-connection-diagram.png)

### <a name="deployment-models-and-methods-for-point-to-site"></a>Modely nasazení a metody pro Point-to-Site

[!INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="V2V"></a>Připojení typu VNet-to-VNet (tunel VPN IPsec/IKE)

Propojení virtuální sítě tooanother virtuální síť (VNet-to-VNet) je podobné tooconnecting umístění lokality tooan místní virtuální síť. Oba typy připojení využívají bránu tooprovide sítě VPN přes zabezpečené tunelové propojení prostřednictvím protokolu IPsec/IKE. Dokonce je možné kombinovat komunikaci typu VNet-to-VNet s konfiguracemi připojení více lokalit. Díky tomu je možné vytvářet topologie sítí, ve kterých se používá propojování více míst i propojování virtuálních sítí.

Hello připojení virtuální sítě může být:

* v hello stejný nebo jiný oblastí
* v hello stejné nebo různých předplatných 
* v hello modely stejný nebo jiný nasazení

![Azure VPN bránu VNet tooVNet připojení příklad](./media/vpn-gateway-about-vpngateways/vpngateway-vnet-to-vnet-connection-diagram.png)

### <a name="connections-between-deployment-models"></a>Připojení mezi různými modely nasazení

Azure v současné době nabízí dva modely nasazení: Classic a Resource Manager. Pokud již Azure nějakou dobu používáte, pravděpodobně vaše virtuální počítače a role instancí Azure fungují ve virtuální síti Classic. Vaše novější virtuální počítače a role instancí však mohou používat virtuální síť vytvořenou v nástroji Resource Manager. Připojení mezi tooallow hello virtuální sítě můžete vytvořit v jedné virtuální sítě toocommunicate přímo s prostředky v jiném hello prostředky.

### <a name="vnet-peering"></a>Partnerské vztahy virtuálních sítí

Možnost toouse VNet peering toocreate připojení, může být také virtuální síti splňuje určité požadavky. VNet peering nepoužívá bránu virtuální sítě. Další informace najdete v tématu [Partnerské vztahy virtuálních sítí](../virtual-network/virtual-network-peering-overview.md).

### <a name="deployment-models-and-methods-for-vnet-to-vnet"></a>Modely nasazení a metody pro VNet-to-VNet

[!INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

## <a name="ExpressRoute"></a>ExpressRoute (vyhrazené soukromé připojení)

Microsoft Azure ExpressRoute umožňuje rozšířit vaše místní sítě do hello cloudu Microsoftu přes vyhrazené soukromé připojení zajišťované poskytovatelem připojení. Prostřednictvím ExpressRoute můžete vytvořit připojení tooMicrosoft cloudové služby, jako je například Microsoft Azure, Office 365 a CRM Online. Co se týká připojení, může se jednat o síť typu any-to-any (IP VPN), síť Ethernet typu point-to-point nebo virtuální křížové připojení prostřednictvím poskytovatele připojení ve společném umístění.

Připojení ExpressRoute se nepřenášejí prostřednictvím hello veřejného Internetu. To umožňuje toooffer připojení ExpressRoute Další spolehlivost, vyšší rychlost, nižší latenci a vyšší zabezpečení než Typická připojení přes hello Internet.

Připojení typu ExpressRoute nepoužívá bránu sítě VPN, přestože brána virtuální sítě využívá jako součást požadované konfigurace. V připojení ExpressRoute Brána virtuální sítě hello nastaven typ brány hello 'ExpressRoute', nikoli 'Vpn'. Další informace o ExpressRoute najdete v tématu hello [technický přehled ExpressRoute](../expressroute/expressroute-introduction.md).

## <a name="coexisting"></a>Současně existující připojení typu Site-to-Site a ExpressRoute

ExpressRoute je přímé vyhrazené připojení z vaší sítě WAN (nikoli prostřednictvím veřejného Internetu hello) tooMicrosoft služeb, včetně Azure. Site-to-Site VPN provozu přenášen zašifrovaně prostřednictvím hello veřejného Internetu. Je možné tooconfigure připojení VPN typu Site-to-Site a ExpressRoute pro hello stejnou virtuální síť má několik výhod.

Můžete nakonfigurovat VPN typu Site-to-Site jako cestu zabezpečené převzetí služeb při selhání pro ExpressRoute, nebo použít toosites tooconnect sítě Site-to-Site VPN, které nejsou součástí vaší sítě, ale jsou připojené prostřednictvím ExpressRoute. Všimněte si, že tato konfigurace vyžaduje dvě brány virtuální sítě pro hello stejné virtuální síti, pomocí hello typ brány, Vpn a pomocí typ brány hello 'ExpressRoute' hello.

![Příklad současné existence ExpressRoute a VPN Gateway](./media/vpn-gateway-about-vpngateways/expressroute-vpngateway-coexisting-connections-diagram.png)

### <a name="deployment-models-and-methods-for-s2s-and-expressroute"></a>Modely nasazení a metody pro S2S a ExpressRoute

[!INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)]

## <a name="pricing"></a>Ceny

[!INCLUDE [vpn-gateway-about-pricing-include](../../includes/vpn-gateway-about-pricing-include.md)]

Další informace o skladových jednotkách (SKU) brány pro službu VPN Gateway najdete v tématu [Skladové jednotky (SKU) brány](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

## <a name="faq"></a>Nejčastější dotazy

Nejčastější dotazy o službě VPN gateway najdete v části hello [VPN Gateway – nejčastější dotazy](vpn-gateway-vpn-faq.md).

## <a name="next-steps"></a>Další kroky

- Plánování konfigurace brány VPN. Viz [Plánování a návrh pro VPN Gateway](vpn-gateway-plan-design.md).
- Zobrazení hello [VPN Gateway – nejčastější dotazy](vpn-gateway-vpn-faq.md) Další informace.
- Zobrazení hello [předplatné a omezení služby](../azure-subscription-service-limits.md#networking-limits).
- Další informace o některých hello Další klíč [sítě možnosti](../networking/networking-overview.md) Azure.
