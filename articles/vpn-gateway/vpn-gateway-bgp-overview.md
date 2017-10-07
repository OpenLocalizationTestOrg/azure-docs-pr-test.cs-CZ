---
title: "aaaOverview protokolu BGP se službou Azure VPN Gateways | Microsoft Docs"
description: "Tento článek obsahuje přehled protokolu BGP se službou Azure VPN Gateways."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: 
ms.assetid: f8c3985c-c128-4f34-835c-0e88742bf36e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/12/2017
ms.author: yushwang
ms.openlocfilehash: ced3f77ecd791c84fb72b96447e839be3bf94846
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-bgp-with-azure-vpn-gateways"></a>Přehled protokolu BGP se službou Azure VPN Gateways
Tento článek obsahuje přehled o podpoře protokolu BGP (Border Gateway Protocol) ve službě Azure VPN Gateways.

BGP je standardní směrovací protokol hello běžně používaný v hello Internet tooexchange směrování a dostupnosti informací mezi dvěma nebo více sítěmi. Kdy použít v kontextu hello virtuálních sítí Azure, protokol BGP povolí hello Azure VPN Gateway a vaše místní zařízení VPN, názvem partnerské uzly protokolu BGP nebo Sousedé BGP, tooexchange "tras", bude informovat obě brány o dostupnosti hello a dostupnosti pro ty předpony toogo prostřednictvím hello bránami nebo trasami související se situací. Protokol BGP můžete také povolit směrování přenosu mezi více sítěmi pomocí šíření tras, které brána s protokolem BGP zjistí od jednoho tooall partnera BGP ostatním partnerům BGP. 

## <a name="why-use-bgp"></a>Proč používat protokol BGP?
Protokol BGP je volitelná funkce, kterou můžete použít s trasovými bránami Azure VPN. Můžete také ujistěte se, že vaše místní zařízení VPN podporuje protokol BGP před povolením funkce hello. Můžete pokračovat v toouse Azure VPN Gateway a vaše místní zařízení VPN bez protokolu BGP. Je hello ekvivalentní používání statických tras (bez protokolu BGP) *oproti* používání dynamického směrování s protokolem BGP mezi vaší sítí a Azure.

Existuje několik výhod a nových schopností při použití protokolu BGP:

### <a name="support-automatic-and-flexible-prefix-updates"></a>Podpora automatických a flexibilních aktualizací předpon
Při použití protokolu BGP musíte pouze toodeclare minimální předponu tooa konkrétní partnerský uzel protokolu BGP přes tunel IPsec S2S VPN hello. Může být malá jako předpona hostitele (/ 32) z hello IP adresy partnerského uzlu protokolu BGP vašeho místního zařízení VPN. Můžete ovládat, který místní předpony sítě chcete tooadvertise tooAzure tooallow tooaccess vaší virtuální síti Azure.

Můžete také inzerovat delší předpony, které mohou obsahovat některé předpony adres vaší virtuální sítě, například velký prostor privátní IP adresy (např. 10.0.0.0/8). Poznámka: když hello předpony nesmí shodovat s žádnými předponami vaší virtuální sítě. Tyto předpony trasy identické tooyour virtuální sítě budou odmítnuty.

### <a name="support-multiple-tunnels-between-a-vnet-and-an-on-premises-site-with-automatic-failover-based-on-bgp"></a>Podpora více tunelů mezi virtuální sítí a místním webem s automatickým převzetím služeb při selhání na základě protokolu BGP
Můžete vytvořit více připojení mezi virtuální sítě Azure a vaše místní zařízení VPN v hello stejné umístění. Tato schopnost poskytuje více tunelů (cest) mezi dvěma sítěmi hello v konfiguraci aktivní aktivní. Pokud jeden z hello tunely není připojen, hello odpovídající trasy se odvolají přes protokol BGP a provoz hello automaticky posune toohello zbývající tunely.

Hello následující diagram ukazuje jednoduchý příklad tohoto vysoce dostupného nastavení:

![Více aktivních cest](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

### <a name="support-transit-routing-between-your-on-premises-networks-and-multiple-azure-vnets"></a>Podpora směrování přenosu mezi místními sítěmi a více virtuálními sítěmi Azure
Protokol BGP umožňuje více bran toolearn a šířit předpony z různých sítí, zda jsou přímo nebo nepřímo připojeny. To umožňuje směrování přenosu pomocí bran Azure VPN mezi vašimi místními weby nebo napříč více službami Azure Virtual Network.

Hello následující diagram ukazuje příklad topologie vícenásobného předávání s více cestami, které mohou přenášet provoz mezi hello dvěma místními sítěmi přes brány Azure VPN v rámci Networks Microsoft hello:

![Vícenásobné předávání přenosu](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## <a name="bgp-faq"></a>PROTOKOL BGP – NEJČASTĚJŠÍ DOTAZY
[!INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)]

## <a name="next-steps"></a>Další kroky
V tématu [Začínáme s protokolem BGP na branách Azure VPN](vpn-gateway-bgp-resource-manager-ps.md) pro kroky tooconfigure protokolu BGP mezi různými místy a připojení VNet-to-VNet.

