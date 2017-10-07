---
title: "Přehled ExpressRoute: Rozšířit vaše místní síť tooAzure přes privátní připojení | Microsoft Docs"
description: "Tento technický přehled ExpressRoute vysvětluje, jak práce připojení ExpressRoute tooextend tooAzure vaší místní síti přes privátní připojení."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
ms.assetid: fd95dcd5-df1d-41d6-85dd-e91d0091af05
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/03/2017
ms.author: cherylmc
ms.openlocfilehash: 01301e1205c12ecdab34dc9d9b92bc7489e7826c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-overview"></a>Přehled ExpressRoute
Microsoft Azure ExpressRoute umožňuje rozšířit vaše místní sítě do hello cloudu Microsoftu přes soukromé připojení zajišťované poskytovatelem připojení. Prostřednictvím ExpressRoute můžete vytvořit připojení tooMicrosoft cloudové služby, jako je například Microsoft Azure, Office 365 a Dynamics 365.

Co se týká připojení, může se jednat o síť typu any-to-any (IP VPN), síť Ethernet typu point-to-point nebo virtuální křížové připojení prostřednictvím poskytovatele připojení ve společném umístění. Připojení ExpressRoute se nepřenášejí prostřednictvím hello veřejného Internetu. To umožňuje toooffer připojení ExpressRoute Další spolehlivost, vyšší rychlost, nižší latenci a vyšší zabezpečení než Typická připojení přes hello Internet. Informace o tom tooconnect tooMicrosoft vaší sítě pomocí služby ExpressRoute, najdete v části [modelů připojení ExpressRoute](expressroute-connectivity-models.md).

![](./media/expressroute-introduction/expressroute-connection-overview.png)

## <a name="key-benefits"></a>Klíčové výhody

* Připojení vrstvy 3 mezi vaší místní sítí a hello cloudu Microsoftu prostřednictvím poskytovatele připojení. Co se týká připojení, může se jednat o síť typu any-to-any (IP VPN), připojení Ethernet typu point-to-point nebo virtuální křížové připojení přes ethernetovou výměnu.
* Připojení tooMicrosoft cloudové služby přes všechny oblasti v geopolitické oblasti hello.
* Globální připojení služby tooMicrosoft přes všechny oblasti s doplňkem ExpressRoute premium.
* Dynamické směrování mezi vaší sítí a Microsoftem prostřednictvím standardních protokolů (BGP).
* Vestavěná redundance v každém umístění partnerského vztahu z důvodu vyšší spolehlivosti.
* Smlouva [SLA](https://azure.microsoft.com/support/legal/sla/) pro provoz připojení.
* Podpora technologie QoS pro Skype pro firmy.

Další informace najdete v tématu hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).

## <a name="features"></a>Funkce

### <a name="layer-3-connectivity"></a>Připojení vrstvy 3
Microsoft používá oborový standard dynamické směrování protocol (BGP) tooexchange tras mezi místní sítí, vašimi instancemi v Azure a Microsoft veřejné adresy.  Navážeme několik relací protokolu BGP s vaší sítí pro různé profily přenosu. Další podrobnosti naleznete v hello [ExpressRoute okruhu a domény směrování](expressroute-circuit-peerings.md) článku.

### <a name="redundancy"></a>Redundance
Každý okruh ExpressRoute sestává ze dvou připojení tootwo hraničním směrovačům Microsoft Enterprise (Msee) od poskytovatele připojení hello / vaší sítě. Microsoft vyžaduje duální připojení BGP z hello poskytovatele připojení nebo vaší strany – jeden tooeach MSEE. Můžete se rozhodnout není toodeploy redundantní zařízení nebo ethernetové okruhy na vaší straně. Poskytovatelé připojení však použít tooensure redundantní zařízení, která vaše připojení jsou předávána tooMicrosoft redundantním způsobem. Konfigurace redundantního připojení vrstvy 3 je požadavkem k naší [SLA](https://azure.microsoft.com/support/legal/sla/) toobe platný.

### <a name="connectivity-toomicrosoft-cloud-services"></a>Připojení tooMicrosoft cloudové služby
[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

Připojení ExpressRoute umožňují přístup toohello následující služby:

* Služby Microsoft Azure
* Služby Microsoft Office 365
* Microsoft Dynamics 365

Můžete taky navštívit hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md) stránky pro podrobný seznam služeb podporovaných přes ExpressRoute.

### <a name="connectivity-tooall-regions-within-a-geopolitical-region"></a>Připojení k tooall oblastem v geopolitické oblasti
Můžete připojit tooMicrosoft v jednom z našich [umístění partnerského vztahu](expressroute-locations.md) a mít přístup tooall oblasti v rámci geopolitické oblasti hello. 

Například pokud jste tooMicrosoft v Amsterdamu připojené prostřednictvím ExpressRoute, máte přístup tooall cloudovým službám Microsoftu hostovaným v oblastech severní Evropa a západní Evropa. V tématu hello [ExpressRoute partnery a umístění partnerského vztahu](expressroute-locations.md) najdete v článku Přehled hello geopolitických oblastí, přidružených oblastí cloudu Microsoftu a odpovídajících umístění partnerského vztahu ExpressRoute.

### <a name="global-connectivity-with-expressroute-premium-add-on"></a>Globální připojení s doplňkem ExpressRoute Premium
Můžete povolit hello ExpressRoute premium rozšíření funkce tooextend připojení přes geopolitické hranice. Například pokud jste tooMicrosoft připojené prostřednictvím ExpressRoute v Amsterdamu, budete mít přístup tooall cloudovým službám Microsoftu hostovaným ve všech oblastech napříč hello, world (národní cloudy jsou vyloučeny). Můžete přistupovat ke službám nasazeným v hello Jižní Amerika nebo Austrálie stejným způsobem, ke kterému přistupujete hello Severní a oblasti západní Evropa.

### <a name="rich-connectivity-partner-ecosystem"></a>Bohatý ekosystém partnerů připojení
ExpressRoute má stále rostoucí ekosystém poskytovatelů připojení a partnerů SI. Můžete se podívat toohello [ExpressRoute poskytovatelé a umístění](expressroute-locations.md) hello nejnovější informace najdete v článku.

### <a name="connectivity-toonational-clouds"></a>Cloudy toonational připojení
Microsoft provozuje izolovaná cloudová prostředí pro speciální geopolitické oblasti a segmenty zákazníků. Odkazovat toohello [ExpressRoute poskytovatelé a umístění](expressroute-locations.md) stránky seznam národních cloudů a poskytovatelů.

### <a name="bandwidth-options"></a>Možnosti šířky pásma
Okruhy ExpressRoute můžete zakoupit pro širokou škálu šířek pásma. Seznam podporovaných šířek pásma je uvedený dál. Musí se toocheck připojení poskytovatele toodetermine hello seznam podporovaných šířek pásma, které poskytují.

* 50 Mb/s
* 100 Mb/s
* 200 Mb/s
* 500 Mb/s
* 1 Gb/s
* 2 Gb/s
* 5 Gb/s
* 10 Gb/s

### <a name="dynamic-scaling-of-bandwidth"></a>Dynamické škálování šířky pásma
Můžete zvýšit hello šířku pásma okruhu ExpressRoute (na kapacita systému dovolí) bez nutnosti tootear dolů připojení. 

### <a name="flexible-billing-models"></a>Flexibilní modely fakturace
Můžete si vybrat fakturační model, který vám nejlépe vyhovuje. Zvolte hello fakturačních modelů uvedených dál. Další informace najdete v tématu hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).

* **Neomezená data** Hello okruh ExpressRoute je účtován na základě měsíčních poplatků a všechny příchozí a odchozí přenosy dat je zahrnuté zdarma. 
* **Měření podle objemu dat**. Hello okruh ExpressRoute je účtován na základě měsíčních poplatků. Všechny příchozí přenosy dat jsou zadarmo. Odchozí přenosy dat se účtují podle přenesených gigabajtů. Sazby za přenos dat se liší podle oblasti.
* **Doplněk ExpressRoute Premium**. Hello ExpressRoute premium je doplněk nad hello okruh ExpressRoute. Hello doplněk ExpressRoute premium poskytuje hello následující možnosti: 
  * Zvýšené limity tras pro veřejný a Azure soukromý partnerský vztah Azure ze 4 000 tras too10, 000 tras.
  * Globální připojení pro služby. Okruh ExpressRoute vytvořený v libovolné oblasti (s výjimkou národních cloudů) bude mít přístup tooresources v libovolné jiné oblasti hello, world. Například virtuální sítě vytvořené v oblasti Západní Evropa budou přístupné prostřednictvím okruhu ExpressRoute zřízeného ze Silicon Valley.
  * Zvýšení počtu propojení virtuálních sítí na jeden okruh ExpressRoute z 10 tooa vyšší limit, v závislosti na hello šířku pásma okruhu hello.

## <a name="faq"></a>Nejčastější dotazy

Nejčastější dotazy o ExpressRoute najdete v části hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).

## <a name="next-steps"></a>Další kroky

* Seznamte se s [modely připojení ExpressRoute](expressroute-connectivity-models.md).
* Přečtěte si další informace o připojeních ExpressRoute a doménách směrování. Viz [Okruhy ExpressRoute a domény směrování](expressroute-circuit-peerings.md).
* Vyhledejte poskytovatele služeb. Viz [Partneři ExpressRoute a umístění partnerského vztahu](expressroute-locations.md).
* Zkontrolujte, že jsou splněné všechny požadavky. Viz [Požadavky služby ExpressRoute](expressroute-prerequisites.md).
* Odkazovat toohello požadavky pro [směrování](expressroute-routing.md), [NAT](expressroute-nat.md), a [QoS](expressroute-qos.md).
* Nakonfigurujte připojení ExpressRoute.
  * [Vytvoření okruhu ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md)
  * [Konfigurace partnerského vztahu pro okruh ExpressRoute](expressroute-howto-routing-portal-resource-manager.md)
  * [Připojit virtuální síť tooan okruh ExpressRoute](expressroute-howto-linkvnet-portal-resource-manager.md)
* Další informace o některých hello Další klíč [sítě možnosti](../networking/networking-overview.md) Azure.
