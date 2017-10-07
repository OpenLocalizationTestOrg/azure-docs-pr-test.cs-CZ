---
title: "aaaAzure ExpressRoute okruhy a domény směrování | Microsoft Docs"
description: "Tato stránka obsahuje přehled okruhy ExpressRoute a domény směrování hello."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
ms.assetid: 6f0c5d8e-cc60-4a04-8641-2c211bda93d9
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/03/2017
ms.author: cherylmc
ms.openlocfilehash: 1d43cbf668accdd7aa4efb053ea1e9027d10e4a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-circuits-and-routing-domains"></a>Okruhy ExpressRoute a domény směrování
 Musíte uspořádat *okruh ExpressRoute* tooconnect vaší místní infrastruktury tooMicrosoft prostřednictvím poskytovatele připojení. Následující obrázek Hello poskytuje vytvoření logického vyjádření připojení mezi vaší sítě WAN a společnosti Microsoft.

![](./media/expressroute-circuit-peerings/expressroute-basic.png)

## <a name="expressroute-circuits"></a>Okruhy ExpressRoute
*Okruh ExpressRoute* představuje logické připojení mezi místní infrastrukturu a cloudových služeb Microsoftu prostřednictvím poskytovatele připojení. Můžete uspořádat více okruhů ExpressRoute. Každý okruh může být v hello stejné nebo různých oblastí, a může být místní připojené tooyour prostřednictvím poskytovatelů jiné připojení. 

Okruhy ExpressRoute nemapovaly tooany fyzických entit. Okruh je jedinečně identifikovaný standardního GUID říká klíče služby (s klíč). klíč služby Hello je jediná hello o výměně informací mezi společností Microsoft, poskytovatel připojení hello a vámi. Hello s klíč je tajný klíč pro účely zabezpečení. Existuje mapování 1:1 mezi okruh ExpressRoute a hello s klíčem.

Okruh ExpressRoute může mít až toothree nezávislé partnerských vztahů: Azure privátní veřejné, Azure a společnosti Microsoft. Každý partnerský vztah je pár nezávislé BGP relací každý z nich redundantně nakonfigurovaný pro vysokou dostupnost. Je 1: n (1 < = N < = 3) mapování mezi okruh ExpressRoute a domény směrování. Okruh ExpressRoute může mít některého, dvě nebo všechny tři partnerské vztahy, které jsou povolené na jeden okruh ExpressRoute.

Každý okruh má pevnou šířku pásma (50 MB/s, 100 MB/s, 200 MB/s, 500 MB/s, 1 GB/s, 10 GB/s) a je poskytovatel připojení namapované tooa a umístění partnerského vztahu. Hello šířky pásma, kterou vyberete, je sdílen na všechny hello partnerských vztahů pro okruh hello. 

### <a name="quotas-limits-and-limitations"></a>Kvót, omezení a omezení
Výchozí kvóty a omezení platí pro každý okruh ExpressRoute. Odkazovat toohello [předplatné Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md) aktuální informace o kvóty.

## <a name="expressroute-routing-domains"></a>Domény směrování ExpressRoute
Okruh ExpressRoute má přidruženo více domény směrování: Azure privátní veřejné, Azure a společnosti Microsoft. Všechny domén směrování hello nakonfigurované stejně jako na pár směrovače (ve sdílení zatížení nebo aktivní aktivní konfigurace) pro zajištění vysoké dostupnosti. Služby Azure jsou klasifikovány jako *Azure veřejné* a *Azure privátní* toorepresent hello schémat adresování IP.

![](./media/expressroute-circuit-peerings/expressroute-peerings.png)

### <a name="private-peering"></a>Soukromý partnerský vztah
Azure výpočetní služby, konkrétně virtuální počítače (IaaS) a cloudové služby (PaaS), které jsou nasazeny v rámci virtuální sítě může být připojen prostřednictvím soukromého partnerského vztahu domény hello. Hello soukromého partnerského vztahu domény je považován za toobe důvěryhodné rozšíření základní sítě do Microsoft Azure. Můžete nastavit obousměrné připojení mezi základní sítě a virtuální sítě Azure (virtuální sítě). Partnerský vztah umožňuje připojit toovirtual počítače a cloudové služby přímo na jejich soukromé IP adresy.  

Více než jedné virtuální sítě toohello soukromého partnerského vztahu domény se můžete připojit. Zkontrolujte hello [stránka s nejčastějšími dotazy](expressroute-faqs.md) informace o omezení. Můžete taky navštívit hello [předplatné Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md) aktuální informace o omezení.  Odkazovat toohello [směrování](expressroute-routing.md) podrobné informace o konfiguraci směrování.

### <a name="public-peering"></a>Veřejný partnerský vztah
Na veřejné IP adresy jsou nabízené služby, například Azure Storage, databáze SQL a weby. Můžete se připojit soukromě tooservices hostované na veřejné IP adresy, včetně virtuální IP adresy z vašich cloudových služeb prostřednictvím hello veřejného partnerského vztahu domény směrování. Můžete se připojit hello veřejného partnerského vztahu domény tooyour hraniční sítě a připojení tooall Azure hello služeb v jejich veřejné IP adresy z vaší sítě WAN bez nutnosti tooconnect prostřednictvím Internetu. 

Připojení je vždycky iniciováno z vaší sítě WAN tooMicrosoft Azure services. Služby Microsoft Azure nebude moct tooinitiate připojení do sítě prostřednictvím této domény směrování. Jakmile veřejného partnerského vztahu je povoleno, bude možné tooconnect tooall Azure services. Můžeme vám nepovolují tooselectively vyberte služby, pro které jsme Inzerovat trasy k. Můžete zkontrolovat hello seznam předpon inzerujeme tooyou prostřednictvím tohoto partnerského vztahu na hello [Microsoft Azure Datacenter rozsahy IP adres](http://www.microsoft.com/download/details.aspx?id=41653) stránky. stránku Hello je aktualizovaný týdně.

V rámci vaší sítě tooconsume pouze hello tras, které potřebujete, můžete definovat vlastní trasy filtry. Odkazovat toohello [směrování](expressroute-routing.md) podrobné informace o konfiguraci směrování. 

V tématu hello [stránka s nejčastějšími dotazy](expressroute-faqs.md) Další informace o služeb podporovaných přes hello veřejného partnerského vztahu domény směrování. 

### <a name="microsoft-peering"></a>Partnerský vztah Microsoftu
[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

Tooall připojení jiných služeb Microsoft online services (například služby Office 365) bude prostřednictvím partnerského vztahu Microsoftu hello. Můžeme povolit obousměrné připojení mezi vaší sítě WAN a cloudové služby Microsoft prostřednictvím hello Microsoft partnerského vztahu domény směrování. TooMicrosoft cloudové služby je nutné připojit pouze přes veřejné IP adresy, které jsou vlastněny vy nebo váš poskytovatel připojení a je nutné splnit tooall hello definovaná pravidla. V tématu hello [požadavky služby ExpressRoute](expressroute-prerequisites.md) stránka Další informace.

V tématu hello [stránka s nejčastějšími dotazy](expressroute-faqs.md) Další informace o služeb podporovaných, náklady a podrobnosti o konfiguraci. V tématu hello [umístění ExpressRoute](expressroute-locations.md) informace o seznamu hello poskytovatelé připojení nabízí podporu partnerského vztahu Microsoftu.

## <a name="routing-domain-comparison"></a>Porovnání směrování domény
Hello tabulka níže porovnává hello tři domény směrování.

|  | **Soukromého partnerského vztahu** | **Veřejný partnerský vztah** | **Partnerský vztah Microsoftu** |
| --- | --- | --- | --- |
| **Max. # předpony podporované na partnerský vztah** |4000 standardně 10 000 s ExpressRoute Premium |200 |200 |
| **Podporované rozsahy IP adres** |Všechny platná adresa IPv4 v rámci vaší sítě WAN. |Vlastníkem vy nebo váš poskytovatel připojení veřejné adresy IPv4. |Vlastníkem vy nebo váš poskytovatel připojení veřejné adresy IPv4. |
| **JAKO počet požadavků** |Privátní a veřejné jako čísla. Musí vlastníte hello veřejné číslo, pokud se rozhodnete toouse jeden. |Privátní a veřejné jako čísla. Však musí prokázat vlastnictví veřejné IP adresy. |Privátní a veřejné jako čísla. Však musí prokázat vlastnictví veřejné IP adresy. |
| **Směrování rozhraní IP adresy** |RFC1918 a veřejné IP adresy |Veřejné IP adresy registrované tooyou v registrech směrování. |Veřejné IP adresy registrované tooyou v registrech směrování. |
| **Hodnota Hash MD5 podpory** |Ano |Ano |Ano |

Jako součást váš okruh ExpressRoute můžete tooenable jeden nebo více domén směrování hello. Můžete zvolit toohave všechny domény směrování hello, které se umístí hello stejné sítě VPN, pokud chcete, aby toocombine je do jedné domény směrování. Můžete je také umístit na různých doménách směrování, podobně jako toohello diagram. Hello doporučená konfigurace této soukromého partnerského vztahu je připojený přímo toohello základní sítě a hello veřejné a připojené tooyour DMZ jsou odkazy partnerského vztahu Microsoft.

Pokud si zvolíte toohave všechny tři relace partnerského vztahu, musí obsahovat tři dvojici relací protokolu BGP (jednu dvojici pro každý typ partnerského vztahu). páry relace protokolu BGP Hello poskytovat vysoce dostupné odkaz. Pokud se připojujete pomocí poskytovatelů vrstvy 2 připojení, můžete je zodpovědná za konfiguraci a správu směrování. Další informace kontrolou hello [pracovních](expressroute-workflows.md) pro nastavení ExpressRoute.

## <a name="next-steps"></a>Další kroky
* Vyhledejte poskytovatele služeb. V tématu [ExpressRoute služby poskytovatelé a umístění](expressroute-locations.md).
* Zkontrolujte, že jsou splněné všechny požadavky. Viz [Požadavky služby ExpressRoute](expressroute-prerequisites.md).
* Nakonfigurujte připojení ExpressRoute.
  * [Vytvoření a správa okruhů ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md)
  * [Konfigurace směrování (partnerského vztahu) pro okruhy ExpressRoute](expressroute-howto-routing-portal-resource-manager.md)

