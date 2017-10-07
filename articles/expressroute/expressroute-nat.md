---
title: "aaaNAT požadavky pro okruhy ExpressRoute | Microsoft Docs"
description: "Tato stránka obsahuje podrobné požadavky pro konfiguraci a správu překladu adres (NAT) pro okruhy ExpressRoute."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: 867bf936-c851-485f-84c8-d8d6e33fee9f
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: cherylmc
ms.openlocfilehash: 09a0e841235de3f6b85e32172d7f99f20b5baf54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-nat-requirements"></a>Požadavky na překlad adres (NAT) služby ExpressRoute
cloudové služby tooMicrosoft tooconnect pomocí služby ExpressRoute, budete potřebovat tooset nahoru a spravovat zařízení NAT. Někteří poskytovatelé připojení nabízejí nastavení a správu NAT jako spravovanou službu. Pokud nabízejí těchto služeb, obraťte se na vaše toosee poskytovatele připojení. Pokud ne, je nutné splnit toohello požadavky popsané dál. 

Zkontrolujte hello [ExpressRoute okruhy a domény směrování](expressroute-circuit-peerings.md) stránka tooget hello přehled různých domén směrování. toomeet hello veřejnou IP adresu požadavky pro veřejný Azure a partnerský vztah Microsoftu, doporučujeme, abyste nastavili NAT mezi vaší sítí a Microsoftem. Tato část obsahuje podrobný popis infrastruktury NAT hello, potřebujete tooset nahoru.

## <a name="nat-requirements-for-azure-public-peering"></a>Požadavky NAT pro veřejný partnerský vztah Azure
Hello cesty veřejného partnerského vztahu Azure umožňuje vám tooconnect tooall služby hostované v Azure přes jejich veřejné IP adresy. Mezi ně patří služby uvedené v hello [Expressroute – nejčastější dotazy](expressroute-faqs.md) a veškeré služby hostované nezávislými dodavateli softwaru v Microsoft Azure. 

> [!IMPORTANT]
> Připojení tooMicrosoft Azure služby na veřejného partnerského vztahu je vždycky iniciováno z vaší sítě do sítě Microsoft hello. Relace nelze inicializovat z sítě tooyour služby Microsoft Azure, tedy přes ExpressRoute. Pokud pokus, inzerované toothese odeslaných paketů IP adresy používat hello internet místo ExpressRoute.
> 

Přenosy určené tooMicrosoft Azure ve veřejném partnerském vztahu musí být vstupem toovalid, veřejné IPv4 adresy před vstupem do sítě Microsoft hello. Následující obrázek Hello poskytuje základní přehled o tom, jak hello NAT může nastavit toomeet hello výše požadavek.

![](./media/expressroute-nat/expressroute-nat-azure-public.png) 

### <a name="nat-ip-pool-and-route-advertisements"></a>Inzerování tras a fondu IP adres NAT
Musíte zajistit, aby provoz vstupující hello veřejné cesty partnerského vztahu Azure měl platnou veřejnou IPv4 adresu. Microsoft musí být schopný toovalidate hello vlastnictví hello fondu IPv4 adres NAT proti regionálnímu registru (RIR) nebo Internetu směrování registru IRR. Kontrola se provede podle hello jako číslo s partnerským a hello adres IP použitých pro hello adres (NAT) Odkazovat toohello [požadavky na směrování služby ExpressRoute](expressroute-routing.md) informace o registrech směrování.

Neexistují žádná omezení délky hello hello předpony adres IP pro NAT inzerovaných prostřednictvím tohoto partnerského vztahu. Musíte monitorovat fond NAT hello a ujistěte se, že nemáte nedostatek relací NAT..

> [!IMPORTANT]
> Hello fond IP adres NAT inzerovaný tooMicrosoft nesmí být inzerovaný toohello Internetu. Tím dojde k přerušení služby Microsoft tooother připojení.
> 
> 

## <a name="nat-requirements-for-microsoft-peering"></a>Požadavky NAT pro partnerský vztah Microsoftu
Cesta partnerského vztahu Microsoftu Hello vám umožní připojit se tooMicrosoft cloudové služby, které nejsou podporované prostřednictvím cesty veřejného partnerského vztahu Azure hello. Hello služby patří služby Office 365, jako je Exchange Online, SharePoint Online, Skype pro firmy a Dynamics 365. Microsoft očekává toosupport obousměrné připojení na partnerský vztah Microsoftu hello. Přenosy určené tooMicrosoft cloudové služby musí být vstupem toovalid, veřejné IPv4 adresy před vstupem do sítě Microsoft hello. Přenosy určené tooyour sítě z cloudových služeb Microsoftu musí být vstupem ve vaší hraniční tooprevent Internet [asymetrické směrování](expressroute-asymmetric-routing.md). Následující obrázek Hello poskytuje základní přehled o tom, jak hello NAT by měla být instalační program pro partnerský vztah Microsoftu.

![](./media/expressroute-nat/expressroute-nat-microsoft.png) 

### <a name="traffic-originating-from-your-network-destined-toomicrosoft"></a>Přenosy pocházející z vaší sítě určené tooMicrosoft
* Musíte zajistit, aby provoz vstupující hello cesty partnerského vztahu Microsoftu měl platnou veřejnou IPv4 adresu. Microsoft musí být schopný toovalidate hello vlastník hello fondu IPv4 adres NAT proti hello regionálnímu registru (RIR) nebo Internetu směrování registru IRR. Kontrola se provede podle hello jako číslo s partnerským a hello adres IP použitých pro hello adres (NAT) Odkazovat toohello [požadavky na směrování služby ExpressRoute](expressroute-routing.md) informace o registrech směrování.
* Použít IP adresy pro hello instalace nástroje Azure veřejného partnerského vztahu a jiné okruhy ExpressRoute nesmí být inzerovaný tooMicrosoft prostřednictvím relace protokolu BGP hello. Neexistuje žádné omezení délky hello hello předpony adres IP pro NAT inzerovaných prostřednictvím tohoto partnerského vztahu.
  
  > [!IMPORTANT]
  > Hello fond IP adres NAT inzerovaný tooMicrosoft nesmí být inzerovaný toohello Internetu. Tím dojde k přerušení služby Microsoft tooother připojení.
  > 
  > 

### <a name="traffic-originating-from-microsoft-destined-tooyour-network"></a>Přenosy pocházející z Microsoftu určené tooyour sítě
* Některé scénáře vyžadují Microsoft tooinitiate připojení tooservice koncové body hostované ve vaší síti. Typickým příkladem takového scénáře hello by připojení tooADFS servery hostované ve vaší síti ze služeb Office 365. V takových případech musíte nechat uniknout příslušné předpony z vaší sítě do partnerského vztahu Microsoft hello. 
* Musí překládat pomocí SNAT Microsoft provoz na hello hraniční sítě Internet pro koncové body služby v rámci vaší sítě tooprevent [asymetrické směrování](expressroute-asymmetric-routing.md). Požadavky **a odpovědi** s cílovou IP adresou, která odpovídá trase obdržené prostřednictvím ExpressRoute, se vždy odešlou přes ExpressRoute. Asymetrické směrování existuje, pokud obdrží požadavek hello prostřednictvím Internetu hello s hello odpověď odeslaná přes ExpressRoute. SNATing hello příchozí Microsoft provoz na hello Internet edge vynutí odpověď provoz back toohello Internet okraji a vyřešení problému hello.

![Asymetrické směrování s ExpressRoute](./media/expressroute-asymmetric-routing/AsymmetricRouting2.png)

## <a name="next-steps"></a>Další kroky
* Odkazovat toohello požadavky pro [směrování](expressroute-routing.md) a [QoS](expressroute-qos.md).
* Informace o pracovním postupu najdete v tématu [Pracovní postupy zřizování okruhů ExpressRoute a stavy okruhu](expressroute-workflows.md).
* Nakonfigurujte připojení ExpressRoute.
  
  * [Vytvoření okruhu ExpressRoute](expressroute-howto-circuit-classic.md)
  * [Konfigurace směrování](expressroute-howto-routing-classic.md)
  * [Propojení virtuální sítě tooan okruh ExpressRoute](expressroute-howto-linkvnet-classic.md)

