---
title: "Modelů připojení ExpressRoute: připojení tooMicrosoft Azure přes poskytovatelů síťových služeb, výměnu a poskytovatelé ethernetových | Microsoft Docs"
description: "Tento článek popisuje různé režimy hello připojení mezi sítí a služby Microsoft Azure, Office 365 a Dynamics 365 hello zákazníka. Zákazníci mohou využívat poskytovatele MPLS, cloudové výměny a poskytovatele ethernetových připojení."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2017
ms.author: cherylmc
ms.openlocfilehash: 2682e6e45b2892869068f132bedb4bb08e3f89a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-connectivity-models"></a>Modely připojení ExpressRoute
Připojení mezi místní sítí a hello cloudu Microsoftu můžete vytvořit třemi různými způsoby, [společné umístění CloudExchange](#CloudExchange), [připojení Ethernet typu Point-to-point](#Ethernet)a [Any-to-any (IPVPN) připojení](#IPVPN). Poskytovatelé připojení můžou nabízet jeden nebo víc modelů připojení. Můžete pracovat s vaše připojení poskytovatele toopick hello model, který nejlépe vyhovuje.
<br><br>

![Diagram modelů připojení ExpressRoute](./media/expressroute-connectivity-models/expressroute-connectivity-models-diagram.png)

## <a name="CloudExchange"></a>Společně umístěné v cloudové výměně
Pokud jsou společně umístění ve budovy s výměnou cloudu, můžete si objednat virtuální křížové připojení toohello cloudu Microsoftu prostřednictvím ethernetové výměny poskytovatele hello společné umístění. Poskytovatelé ve společném umístění můžou nabízet křížová připojení vrstvy 2 nebo spravovaná připojení vrstvy 3 mezi-mezi vaší infrastrukturou ve společném umístění hello a cloudu Microsoft hello.

## <a name="Ethernet"></a>Ethernetová připojení typu point-to-point
Můžete se připojit vaše místní datová centra nebo pobočky toohello cloudu Microsoftu prostřednictvím ethernetových propojení typu point-to-point. Poskytovatelé sítě Ethernet typu Point-to-Point můžou nabízet připojení vrstvy 2 nebo spravovaná připojení vrstvy 3 mezi vaší lokalitou a hello cloudu Microsoftu.

## <a name="IPVPN"></a>Sítě typu any-to-any (IPVPN)
S hello cloudu Microsoftu můžete integrovat vaši síť WAN. Poskytovatelé IPVPN (obvykle MPLS VPN) nabízejí připojení typu any-to-any mezi pobočkami a datovými centry. Hello Microsoft cloudu lze vzájemně propojena tooyour WAN toomake ho vypadat stejně jako jiné pobočky. Poskytovatelé sítě WAN obvykle nabízejí spravované připojení vrstvy 3. Funkce a možnosti ExpressRoute jsou všechny stejné napříč všemi hello výše modelů připojení. 

## <a name="next-steps"></a>Další kroky
* Přečtěte si další informace o připojeních ExpressRoute a doménách směrování. Viz [Okruhy ExpressRoute a domény směrování](expressroute-circuit-peerings.md).
* Seznamte se s funkcemi ExpressRoute. V tématu hello [technický přehled ExpressRoute](expressroute-introduction.md)
* Vyhledejte poskytovatele služeb. Viz [Partneři ExpressRoute a umístění partnerského vztahu](expressroute-locations.md).
* Zkontrolujte, že jsou splněné všechny požadavky. Viz [Požadavky služby ExpressRoute](expressroute-prerequisites.md).
* Odkazovat toohello požadavky pro [směrování](expressroute-routing.md), [NAT](expressroute-nat.md), a [QoS](expressroute-qos.md).
* Nakonfigurujte připojení ExpressRoute.
  * [Vytvoření okruhu ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md)
  * [Konfigurace směrování](expressroute-howto-routing-portal-resource-manager.md)
  * [Propojení virtuální sítě tooan okruh ExpressRoute](expressroute-howto-linkvnet-portal-resource-manager.md)
