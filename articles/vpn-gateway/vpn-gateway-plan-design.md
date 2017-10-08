---
title: "Plánování a návrhu pro připojení mezi různými místy: Azure VPN Gateway | Microsoft Docs"
description: "Další informace o službě VPN Gateway plánování a návrh mezi různými místy, hybridního i připojení VNet-to-VNet"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: d5aaab83-4e74-4484-8bf0-cc465811e757
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/27/2017
ms.author: cherylmc
ms.openlocfilehash: 3d4587ba31d163384212eca88a7e2c0ba8f3b21f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="planning-and-design-for-vpn-gateway"></a>Plánování a návrh pro VPN Gateway

Plánování a návrhu mezi různými místy a VNet-to-VNet konfigurace může být jednoduchý nebo složitá, v závislosti na vašich sítí. Tento článek vás provede základní aspekty plánování a návrhu.

## <a name="planning"></a>Plánování

### <a name="compare"></a>Možnosti připojení mezi různými místy

Pokud chcete tooconnect místní lokality bezpečně tooa virtuální sítě, takže máte tři různé způsoby toodo: Site-to-Site, Point-to-Site a ExpressRoute. Porovnejte hello různých mezi různými místy připojení, které jsou k dispozici. možnost Hello může záviset na různé aspekty, jako třeba:

* Jaký druh propustnosti vyžaduje vaše řešení?
* Chcete toocommunicate přes hello veřejný Internet prostřednictvím bezpečné VPN nebo přes privátní připojení?
* Máte veřejných IP adres k dispozici toouse?
* Pokud plánujete toouse zařízení VPN? Pokud ano, je kompatibilní?
* Připojujete pouze několik počítačů, nebo chcete trvalé připojení pro svůj server?
* Jaký typ brány VPN je požadované pro řešení hello chcete toocreate?
* Které skladová položka brány mám použít?

### <a name="planningtable"></a>Plánovací tabulka

Hello následující tabulka vám může pomoct rozhodnout hello nejlepší možnosti připojení pro vaše řešení.

[!INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]

### <a name="gwsku"></a>SKU brány

[!INCLUDE [vpn-gateway-table-gwtype-aggtput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

### <a name="wf"></a>Pracovní postup

Hello následující seznam obsahuje přehled hello běžné pracovní postup pro připojení k cloudu:

1. Návrh a plánování, že vaše připojení topologie a seznamu Adresa hello prostory pro všechny sítě chcete tooconnect.
2. Vytvoření virtuální sítě Azure. 
3. Vytvořte bránu sítě VPN pro virtuální síť hello.
4. Vytvořit a nakonfigurovat připojení tooon místní sítě nebo jiné virtuální sítě (podle potřeby).
5. Vytvořit a nakonfigurovat připojení Point-to-Site pro bránu Azure VPN (podle potřeby).

## <a name="design"></a>Návrh
### <a name="topologies"></a>Topologie připojení ke službě

Začněte tím, že prohlížení hello diagramů v hello [o službě VPN Gateway](vpn-gateway-about-vpngateways.md) článku. Hello článek obsahuje základní diagramy, hello modely nasazení pro každou topologie a nástroje pro nasazení k dispozici hello toodeploy můžete použít konfiguraci.

### <a name="designbasics"></a>Základní informace o návrhu

Hello následující oddíly popisují základy brány VPN hello. 

#### <a name="servicelimits"></a>Omezení služby sítě

Procházení hello tabulky tooview [sítě služby omezení](../azure-subscription-service-limits.md#networking-limits). omezení Hello uvedené může mít vliv na návrh vašeho.

#### <a name="subnets"></a>O podsítě

Při vytváření připojení, musíte zvážit vaší rozsahy podsítě. Nemůže mít překrývající se rozsahy adres podsítě. Překrývající se podsítí je, když jeden virtuální síti nebo na místní umístění obsahuje hello, který obsahuje stejný adresní prostor, který hello jiného umístění. To znamená, třeba technici vaší sítě pro vaše místní sítě toocarve se rozsah adres pro toouse můžete pro Azure na IP adresování místa nebo podsítě. Je nutné adresní prostor, který se nepoužívá v místní síti hello.

Zamezení překrývající se podsítí je také důležité, když pracujete s připojení VNet-to-VNet. Pokud podsítě překrývajících se a IP adresu v odesílání hello i cílové virtuální sítě existuje, připojení VNet-to-VNet se nezdaří. Azure nelze směrovat hello data toohello jiné virtuální sítě, protože hello cílová adresa je součástí hello odesílání virtuální sítě.

Brány sítě VPN vyžaduje konkrétní podsíť s názvem podsíť brány. Všechny podsítě brány musí mít název GatewaySubnet toowork správně. Ujistěte se, není tooname podsítě brány na jiný název a nenasazujte virtuální počítače ani cokoli jiného toohello podsíť brány. V tématu [podsítě brány](vpn-gateway-about-vpn-gateway-settings.md#gwsub).

#### <a name="local"></a>O brány místní sítě

Hello brány místní sítě obvykle odkazuje tooyour místní umístění. V modelu nasazení classic hello je brána místní sítě hello odkazované tooas místní síťové lokality. Pokud budete konfigurovat bránu místní sítě, zadejte jeho název, zadejte hello veřejná IP adresa zařízení VPN místní hello a zadejte hello předpony, které se nacházejí v umístění místní hello. Azure zjistí hello předpony cílových adres pro síťový provoz, zajímají hello konfigurace, který jste zadali pro bránu místní sítě hello a směruje pakety odpovídajícím způsobem. Předpony adres hello můžete upravit podle potřeby. Další informace najdete v tématu [brány místní sítě](vpn-gateway-about-vpn-gateway-settings.md#lng).

#### <a name="gwtype"></a>O typech brány

Výběr typu hello správné brány pro vaše topologie je velmi důležité. Pokud vyberete hello chybný typ, brána nebude fungovat správně. Typ brány Hello Určuje, jak samotné bráně hello připojí a je požadované nastavení pro model nasazení Resource Manager hello.

Hello brány typy jsou:

* Vpn
* ExpressRoute

#### <a name="connectiontype"></a>O typech připojení

Každá konfigurace vyžaduje určitý typ připojení. Hello typy připojení jsou:

* Protokol IPsec
* Vnet2Vnet
* ExpressRoute
* VPNClient

#### <a name="vpntype"></a>O typy sítě VPN

Každá konfigurace vyžaduje určitý typ sítě VPN. Pokud kombinujete dvě konfigurace, jako je například vytváření připojení Site-to-Site a toohello připojení Point-to-Site stejné virtuální síti, musíte použít typ sítě VPN, který splňuje požadavky obou připojení.

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

Hello následující tabulky popisují typ sítě VPN hello jako mapuje tooeach konfigurace připojení. Ujistěte se, zda text hello typ sítě VPN pro konfiguraci brány odpovídá hello, které chcete toocreate. 

[!INCLUDE [vpn-gateway-table-vpntype](../../includes/vpn-gateway-table-vpntype-include.md)]

### <a name="devices"></a>Zařízení VPN pro připojení Site-to-Site

připojení tooconfigure Site-to-Site, bez ohledu na modelu nasazení, je třeba hello následující položky:

* Zařízení VPN, který je kompatibilní s Azure VPN Gateway
* Veřejnou adresu IPv4 IP, který není za zařízení NAT

Budete potřebovat prostředí toohave konfiguraci zařízení VPN, nebo požádat uživatele, který může nakonfigurovat zařízení hello za vás.

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

### <a name="forcedtunnel"></a>Vezměte v úvahu vynucené tunelového propojení směrování

Pro většinu konfiguraci můžete konfigurovat vynucené tunelování. Vynutit tunelové propojení umožňuje přesměrování nebo "Vynutit" všechny internetový provoz back tooyour místní umístění prostřednictvím tunelu Site-to-Site VPN pro kontrolu a auditování. Požadavek kritické zabezpečení pro většinu organizace IT zásad. 

Bez vynucené tunelování, internetový provoz z virtuálních počítačů v Azure bude vždy procházení od Azure síťové infrastruktury přímo na Internetu, toohello bez tooallow možnost hello je provoz hello tooinspect nebo kontrola. Neoprávněný přístup k Internetu může potenciálně vést tooinformation zpřístupnění nebo jiné typy narušení zabezpečení.

V obou modelech nasazení a pomocí jiných nástrojů se dá konfigurovat vynucené tunelové připojení. Další informace najdete v tématu [konfigurace vynuceného tunelování](vpn-gateway-forced-tunneling-rm.md).

**Vynutit tunelové diagram**

![Vynutit tunelové diagram služby Azure VPN Gateway](./media/vpn-gateway-plan-design/forced-tunneling-diagram.png)

## <a name="next-steps"></a>Další kroky

V tématu hello [VPN Gateway – nejčastější dotazy](vpn-gateway-vpn-faq.md) a [o službě VPN Gateway](vpn-gateway-about-vpngateways.md) články pro další informace o toohelp jste s návrhu.

Další informace o nastavení konkrétní brány najdete v tématu [o nastavení brány sítě VPN](vpn-gateway-about-vpn-gateway-settings.md).