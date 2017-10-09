---
title: "aaaAzure ExpressRoute – nejčastější dotazy | Microsoft Docs"
description: "Hello ExpressRoute – nejčastější dotazy obsahuje informace o podporované služby Azure, náklady, dat a připojení, SLA, poskytovatelé a umístění, šířky pásma a další technické podrobnosti."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
ms.assetid: 09b17bc4-d0b3-4ab0-8c14-eed730e1446e
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: c01e83f1497103e2fa85251dce6fb41844e46e9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-faq"></a>ExpressRoute – nejčastější dotazy

## <a name="what-is-expressroute"></a>Co je ExpressRoute?

ExpressRoute je služba Azure, která umožňuje vytvářet privátní připojení mezi datovými centry společnosti Microsoft a infrastruktury, který je místně nebo v budovy společné umístění. Připojení ExpressRoute se nepřenášejí prostřednictvím veřejného Internetu a nabídka vyšší zabezpečení, spolehlivost hello a rychlosti s nižší latenci než Typická připojení přes hello Internet.

### <a name="what-are-hello-benefits-of-using-expressroute-and-private-network-connections"></a>Jaké jsou výhody hello pomocí ExpressRoute a připojení k privátní síti?

Připojení ExpressRoute se nepřenášejí prostřednictvím hello veřejného Internetu. Nabízí vyšší zabezpečení, spolehlivost a rychlosti, s konzistentní a nižší latenci než Typická připojení přes hello Internet. V některých případech pomocí dat tootransfer připojení ExpressRoute mezi místní zařízení a Azure přispět výraznému snížení nákladů.

### <a name="where-is-hello-service-available"></a>Kde je k dispozici hello služby?

Zobrazí tato stránka pro umístění služby a dostupnosti: [ExpressRoute partnery a umístění](expressroute-locations.md).

### <a name="how-can-i-use-expressroute-tooconnect-toomicrosoft-if-i-dont-have-partnerships-with-one-of-hello-expressroute-carrier-partners"></a>Jak můžete použít ExpressRoute tooconnect tooMicrosoft, pokud není k dispozici partnerství s jedním z partnerů hello poskytovatel ExpressRoute?

Můžete vybrat místní poskytovatel a zobrazovat tooone připojení Ethernet Exchange hello podporované umístění poskytovatele. Můžete pak partnerský vztah Microsoftu na adresu hello umístění poskytovatele. Zkontrolujte poslední část hello [ExpressRoute partnery a umístění](expressroute-locations.md) toosee, pokud se nachází v žádném hello exchange umístění poskytovatele služeb. Potom můžete objednat okruh ExpressRoute prostřednictvím tooAzure tooconnect poskytovatele služby hello.

### <a name="how-much-does-expressroute-cost"></a>Kolik stojí ExpressRoute?

Zkontrolujte [podrobnosti o cenách](https://azure.microsoft.com/pricing/details/expressroute/) informace o cenách.

### <a name="if-i-pay-for-an-expressroute-circuit-of-a-given-bandwidth-does-hello-vpn-connection-i-purchase-from-my-network-service-provider-have-toobe-hello-same-speed"></a>Pokud I platit pro okruh ExpressRoute danou šířku pásma, hello připojení VPN, které zakoupit od svého poskytovatele síťové služby mají toobe hello stejné rychlost?

Ne. Připojení k síti VPN žádné rychlostí si můžete zakoupit od vašeho poskytovatele služeb. Vaše připojení tooAzure je však omezená toohello šířku pásma okruhu ExpressRoute, který jste si koupili.

### <a name="if-i-pay-for-an-expressroute-circuit-of-a-given-bandwidth-do-i-have-hello-ability-tooburst-up-toohigher-speeds-if-necessary"></a>Pokud platí I pro okruh ExpressRoute dané šířky pásma, je nutné provést hello možnost tooburst až toohigher rychlosti v případě potřeby?

Ano. Okruhy ExpressRoute jsou nakonfigurované tooallow tooburst tootwo časů hello limit šířky pásma, který jste si opatřili pro bez dalších nákladů. Pokud se podporu této možnosti, obraťte se na vaše toosee zprostředkovatele služby.

### <a name="can-i-use-hello-same-private-network-connection-with-virtual-network-and-other-azure-services-simultaneously"></a>Můžete použít hello stejné privátní současně síťové připojení pomocí virtuální sítě a dalším službám Azure?

Ano. Okruh ExpressRoute, po nastavení, můžete tooaccess služby v rámci virtuální sítě a dalším službám Azure současně. Toovirtual sítě se připojují přes hello cestou soukromého partnerského vztahu a tooother služby přes hello cesty veřejného partnerského vztahu.

### <a name="does-expressroute-offer-a-service-level-agreement-sla"></a>Nabízí ExpressRoute smlouvy úroveň služeb (SLA)?

Informace najdete v tématu hello [ExpressRoute SLA](https://azure.microsoft.com/support/legal/sla/) stránky.

## <a name="supported-services"></a>Podporované služby

ExpressRoute podporuje [tři domény směrování](expressroute-circuit-peerings.md) pro různé typy služeb.

### <a name="private-peering"></a>Soukromý partnerský vztah

* Virtuální sítě, včetně všech virtuálních počítačů a cloudové služby

### <a name="public-peering"></a>Veřejný partnerský vztah

* Power BI
* Dynamics 365 Finance a operace (dříve označované jako Dynamics AX Online)
* Většina hello Azure services s hello následující několika výjimkami:
  * CDN
  * Visual Studio Team Services zátěžové testování
  * Multi-factor Authentication
  * Traffic Manager

### <a name="microsoft-peering"></a>Partnerský vztah Microsoftu

* [Office 365](http://aka.ms/ExpressRouteOffice365)
* Aplikace Dynamics 365 zákaznické Engagement (dříve označované jako CRM Online)
  * Dynamics 365 pro prodej
  * Dynamics 365 zákaznický servis
  * Dynamics 365 služby pole
  * Dynamics 365 pro – služba projektu

## <a name="data-and-connections"></a>Data a připojení

### <a name="are-there-limits-on-hello-amount-of-data-that-i-can-transfer-using-expressroute"></a>Existují omezení na hello množství dat, který může přenést pomocí ExpressRoute?

Omezení jsme na hello objem přenosu dat není nastaven. Odkazovat příliš[podrobnosti o cenách](https://azure.microsoft.com/pricing/details/expressroute/) informace o šířku pásma sazby.

### <a name="what-connection-speeds-are-supported-by-expressroute"></a>ExpressRoute podporuje rychlost co připojení?

Podporované šířky pásma nabízí:

50 Mb/s, 100 Mb/s, 200 Mb/s, 500 Mb/s, 1 Gb/s, 2 Gb/s, 5 Gb/s, 10 Gb/s

### <a name="which-service-providers-are-available"></a>Které poskytovatelé jsou k dispozici?

V tématu [ExpressRoute partnery a umístění](expressroute-locations.md) hello seznam poskytovatelé a umístění.

## <a name="technical-details"></a>Technické podrobnosti

### <a name="what-are-hello-technical-requirements-for-connecting-my-on-premises-location-tooazure"></a>Jaké jsou hello technické požadavky pro připojení Moje tooAzure místní umístění?

V tématu [stránky požadavky ExpressRoute](expressroute-prerequisites.md) požadavky.

### <a name="are-connections-tooexpressroute-redundant"></a>Jsou redundantní připojení tooExpressRoute?

Ano. Každý okruh ExpressRoute má redundantní dvojici křížové připojení nakonfigurovaná tooprovide vysokou dostupnost.

### <a name="will-i-lose-connectivity-if-one-of-my-expressroute-links-fail"></a>Bude ztrátě připojení, pokud jeden z odkazů Moje ExpressRoute?

Pokud nedojde ke ztrátě připojení z hello křížové připojení selže. Redundantní připojení je k dispozici toosupport hello zatížení sítě. Kromě toho můžete vytvořit více okruhů v různých odolnost selhání tooachieve partnerského vztahu umístění.

### <a name="onep2plink"></a>Pokud nejste společně umístěné na výměně cloudu a poskytovateli služeb nabízí připojení typu point-to-point, je nutné tooorder dvě fyzického připojení mezi Moje místní sítí a Microsoftem?

Pokud váš poskytovatel služeb mohou vytvořit dva okruhy virtuální sítě Ethernet přes hello fyzické připojení, potřebujete jenom jedno fyzické připojení. Hello fyzické (například optického vlákna) je připojení ukončeno ve vrstvě 1 (Ú1) zařízení (viz obrázek hello). okruhy virtuální sítě Ethernet Hello dva jsou označené jiné ID sítě VLAN, jeden pro hello primární okruh a jeden pro sekundární hello. Tyto identifikátory ID sítě VLAN jsou v záhlaví Ethernet hello vnější 802.1Q. záhlaví Hello vnitřní 802.1Q Ethernet (není vidět) je namapované tooa konkrétní [domény směrování ExpressRoute](expressroute-circuit-peerings.md).

![](./media/expressroute-faqs/expressroute-p2p-ref-arch.png)

### <a name="can-i-extend-one-of-my-vlans-tooazure-using-expressroute"></a>Je možné rozšířit jeden z mých sítí VLAN tooAzure pomocí služby ExpressRoute?

Ne. Nepodporujeme rozšíření vrstvy 2 připojení do Azure.

### <a name="can-i-have-more-than-one-expressroute-circuit-in-my-subscription"></a>Může mít více než jeden okruh ExpressRoute v Moje předplatné?

Ano. Můžete mít více než jeden okruh ExpressRoute v rámci vašeho předplatného. Hello výchozí limit je nastaven too10. Microsoft Support tooincrease hello limit, obraťte se v případě potřeby.

### <a name="can-i-have-expressroute-circuits-from-different-service-providers"></a>Může mít okruhů ExpressRoute z různé poskytovatele služeb?

Ano. Okruhy ExpressRoute může mít s mnoho poskytovatelů služeb. Každý okruh ExpressRoute je přidružen pouze jedna služba Zprostředkovatel. 

### <a name="can-i-have-multiple-expressroute-circuits-in-hello-same-location"></a>Může mít více okruhů ExpressRoute v hello stejné umístění?

Ano. Můžete mít více okruhů ExpressRoute, s hello stejné nebo různé poskytovatele služeb v hello stejné umístění. Však nemůže propojit více než jeden toohello okruh ExpressRoute stejné virtuální sítě z hello stejné umístění.

### <a name="how-do-i-connect-my-virtual-networks-tooan-expressroute-circuit"></a>Postup připojení Můj virtuální sítě tooan okruh ExpressRoute

Toto jsou základní kroky Hello:

* Vytvoření okruhu ExpressRoute a poskytovatele služeb hello ji povolte.
* Jste nebo hello poskytovatele, musíte nakonfigurovat hello BGP partnerského vztahu (s).
* Propojení okruhu ExpressRoute toohello hello virtuální sítě.

Další informace najdete v tématu [zřizování okruhů a stavy okruhů ve workflowech ExpressRoute](expressroute-workflows.md).

### <a name="are-there-connectivity-boundaries-for-my-expressroute-circuit"></a>Existují připojení hranice pro okruh ExpressRoute?

Ano. Hello [ExpressRoute partnery a umístění](expressroute-locations.md) článek obsahuje přehled hello připojení hranice pro okruh ExpressRoute. Připojení pro okruh ExpressRoute je omezená tooa jedné geopolitické oblasti. Připojení může být rozšířené toocross geopolitické oblasti povolením hello ExpressRoute premium funkce.

### <a name="can-i-link-toomore-than-one-virtual-network-tooan-expressroute-circuit"></a>Můžete propojit toomore než jednu virtuální síť tooan okruh ExpressRoute?

Ano. Můžete mít too10 připojení virtuální sítě na standardní okruh ExpressRoute a až too100 na [okruh ExpressRoute premium](#expressroute-premium). 

### <a name="i-have-multiple-azure-subscriptions-that-contain-virtual-networks-can-i-connect-virtual-networks-that-are-in-separate-subscriptions-tooa-single-expressroute-circuit"></a>Mám několik předplatných Azure, které obsahují virtuální sítě. Je možné připojit virtuální sítě, které jsou v samostatných odběry tooa jeden okruh ExpressRoute?

Ano. Autorizujete až too10 jiných předplatných Azure toouse jeden okruh ExpressRoute. Toto omezení můžete zvýšit povolením hello ExpressRoute premium funkce.

Další informace najdete v tématu [sdílení okruh ExpressRoute napříč více předplatných](expressroute-howto-linkvnet-arm.md).

### <a name="are-virtual-networks-connected-toohello-same-circuit-isolated-from-each-other"></a>Jsou virtuální sítě připojený toohello stejnému okruhu izolované od sebe navzájem?

Ne. Z směrování propojené toohello perspektivy, všechny virtuální sítě stejnému okruhu ExpressRoute jsou součástí stejné domény směrování hello a nejsou od sebe navzájem oddělené. Pokud třeba směrovat izolace, je třeba toocreate samostatné okruh ExpressRoute.

### <a name="can-i-have-one-virtual-network-connected-toomore-than-one-expressroute-circuit"></a>Může mít jeden virtuální sítě připojený toomore než jeden okruh ExpressRoute?

Ano. Můžete propojit jedné virtuální sítě s až toofour okruhy ExpressRoute. Musejí být seřazeny prostřednictvím čtyři různé [umístění ExpressRoute](expressroute-locations.md).

### <a name="can-i-access-hello-internet-from-my-virtual-networks-connected-tooexpressroute-circuits"></a>Můžete přístup hello Internetu z mé okruhy tooExpressRoute připojené virtuální sítě?

Ano. Pokud nebyly inzerované výchozí trasy (0.0.0.0/0) nebo předpony trasy Internet prostřednictvím relace protokolu BGP hello, je možné připojit toohello Internetu z virtuální sítě propojené tooan okruh ExpressRoute.

### <a name="can-i-block-internet-connectivity-toovirtual-networks-connected-tooexpressroute-circuits"></a>Můžete blokovat internetové připojení toovirtual sítě připojené tooExpressRoute okruhy?

Ano. Můžete inzerování výchozí trasy (0.0.0.0/0) tooblock všechny počítače toovirtual připojení k Internetu nasazuje v rámci virtuální sítě a směrovat všechny přenosy se prostřednictvím okruhu ExpressRoute hello.

Pokud jste prezentovat výchozí směrování, jsme vynutit nabízí prostřednictvím veřejného partnerského vztahu (například úložiště Azure a databázi SQL) místní back tooyour tooservices provoz. Tooconfigure bude mít vaše směrovače tooreturn provoz tooAzure prostřednictvím cesty veřejného partnerského vztahu hello nebo přes hello Internet.

### <a name="can-virtual-networks-linked-toohello-same-expressroute-circuit-talk-tooeach-other"></a>Můžete virtuální sítě propojené toohello stejnému okruhu ExpressRoute komunikovat tooeach další?

Ano. Virtuálních počítačů nasazených v připojené toohello virtuální sítě, které stejnému okruhu ExpressRoute můžete vzájemně komunikovat.

### <a name="can-i-use-site-to-site-connectivity-for-virtual-networks-in-conjunction-with-expressroute"></a>Můžete použít připojení site-to-site pro virtuální sítě ve spojení s ExpressRoute?

Ano. ExpressRoute mohou existovat vedle sebe sítě site-to-site VPN.

### <a name="can-i-move-a-virtual-network-from-site-to-site--point-to-site-configuration-toouse-expressroute"></a>Můžete přesunout virtuální sítě z konfigurace site-to-site / point-to-site toouse ExpressRoute?

Ano. Budete mít toocreate brány ExpressRoute v rámci virtuální sítě. Existuje malé výpadky související s procesem hello.

### <a name="why-is-there-a-public-ip-address-associated-with-hello-expressroute-gateway-on-a-virtual-network"></a>Proč se veřejné IP adresy přidružené k hello ExpressRoute bránu virtuální sítě?

Hello veřejná IP adresa se používá pro interní správu jenom. Tato veřejná IP adresa není zveřejněné toohello Internet a nepředstavuje ohrožení zabezpečení vaší virtuální sítě.

### <a name="what-do-i-need-tooconnect-tooazure-storage-over-expressroute"></a>Co dělat potřebuji tooconnect tooAzure úložiště přes ExpressRoute?

Musíte vytvořit okruh ExpressRoute a nakonfigurovat trasy pro veřejný partnerský vztah.

### <a name="are-there-limits-on-hello-number-of-routes-i-can-advertise"></a>Existují omezení počtu hello tras, které můžete umisťovat reklamy?

Ano. Můžeme přijmout too4000 předpony trasy pro soukromý partnerský vztah a 200 veřejného partnerského vztahu a partnerského vztahu Microsoftu. Tato too10 může zvýšit 000 tras pro soukromý partnerský vztah, pokud povolíte hello ExpressRoute premium funkce.

### <a name="are-there-restrictions-on-ip-ranges-i-can-advertise-over-hello-bgp-session"></a>Existují omezení IP adres I můžete inzerovat přes relaci protokolu BGP hello?

Jsme nepřijímají privátní předpony (RFC1918) v hello veřejné a Microsoft relaci partnerského vztahu protokolu BGP.

### <a name="what-happens-if-i-exceed-hello-bgp-limits"></a>Co se stane, když být delší než omezení hello protokolu BGP?

Relace protokolu BGP se zahodí. Bude resetován po hello předponu počet přejde pod hello limit.

### <a name="what-is-hello-expressroute-bgp-hold-time-can-it-be-adjusted"></a>Co je hello doba uchování ExpressRoute BGP? Lze jej upravit?

Doba uchování Hello je 180. zprávy keep-alive Hello odešlou každých 60 sekund. Tyto jsou pevné nastavení hello straně společnosti Microsoft, kterou nelze změnit. Je možné pro vás jiné časovače tooconfigure a bude odpovídajícím způsobem vyjednávat parametry relace protokolu BGP hello.

### <a name="after-i-advertise-hello-default-route-00000-toomy-virtual-networks-i-cant-activate-windows-running-on-my-azure-vms-how-tooi-fix-this"></a>Po I inzerovat hello výchozí trasa (0.0.0.0/0) toomy virtuální sítě, nelze aktivovat systém Windows spuštěn na můj virtuálních počítačích Azure. Jak tooI tento problém odstranit?

Azure rozpoznat žádost o aktivaci hello pomohli s Hello následující kroky:

1. Vytvořte hello veřejný partnerský vztah pro váš okruh ExpressRoute.
2. Provádět vyhledávání DNS a najít IP adresu hello **kms.core.windows.net**
3. Hello služby správy klíčů musí rozpoznat tuto žádost o aktivaci hello pochází z Azure a dodržet hello požadavku. Proveďte jeden z následujících tří úlohy hello:

   * V místní síti směrovat hello provoz určený pro hello IP adresu, kterou jste získali v kroku 2 back tooAzure prostřednictvím veřejného partnerského vztahu hello.
   * Máte vaše NSP zprostředkovatele kříž pin hello provoz back tooAzure prostřednictvím veřejného partnerského vztahu hello.
   * Vytvoření hello IP adresa tohoto bodů, která má Internetu jako další segment trasy definované uživatelem a jeho použití toohello podsítí, kde jsou tyto virtuální počítače.

### <a name="can-i-change-hello-bandwidth-of-an-expressroute-circuit"></a>Můžete změnit hello šířku pásma okruhu ExpressRoute?

Ano, můžete se pokusit tooincrease hello šířka pásma okruhu ExpressRoute v hello portál Azure nebo pomocí prostředí PowerShell. Pokud na hello fyzický port na kterém byla vytvořena váš okruh je dostupná kapacita, bude úspěšná změna. 

Pokud změny selže, znamená to není k dispozici dostatek kapacita na aktuální port hello vlevo a potřebujete toocreate nové okruh ExpressRoute s větší šířku pásma hello nebo že je v tomto umístění žádné další kapacitu, v takovém případě nebudete moct tooincrease hello šířky pásma. 

Bude také nutné toofollow s vaší tooensure zprostředkovatele připojení k aktualizaci hello omezení v rámci zvýšení jejich sítě hello toosupport šířky pásma. Nelze, ale snížit hello šířka pásma okruhu ExpressRoute. Máte toocreate nové okruh ExpressRoute s menší šířkou pásma a odstranit staré okruh hello.

### <a name="how-do-i-change-hello-bandwidth-of-an-expressroute-circuit"></a>Změna hello šířku pásma okruhu ExpressRoute

Můžete aktualizovat hello šířku pásma okruhu ExpressRoute hello pomocí hello rozhraní API REST nebo rutiny prostředí PowerShell.

## <a name="expressroute-premium"></a>ExpressRoute premium

### <a name="what-is-expressroute-premium"></a>Co je ExpressRoute premium?

ExpressRoute premium je kolekce hello následující funkce:

* Zvýšit limit směrovací tabulky z too10 4000 směrování, 000 tras pro soukromý partnerský vztah.
* Zvýšit počet virtuálních sítí, které můžou být připojené toohello okruh ExpressRoute (výchozí hodnota je 10). Další informace najdete v tématu hello [ExpressRoute omezení](#limits) tabulky.
* Připojení tooOffice 365 a Dynamics 365.
* Globální připojení přes hello Microsoft základní sítě. Nyní můžete propojit virtuální síť v geopolitické oblasti jeden okruh ExpressRoute v jiné oblasti.<br>
    **Příklady:**

    *  Můžete propojit virtuální sítě vytvořené v západní Evropa tooan okruh ExpressRoute vytvořené v Silicon Valley. 
    *  Na hello veřejného partnerského vztahu, jsou inzerované předpony z jiných geopolitických oblastí, tak, aby se můžete připojit k, například SQL Azure v západní Evropa z okruhu ze Silicon Valley.


### <a name="limits"></a>Tom, kolik virtuálních sítí můžete propojit okruh ExpressRoute tooan Pokud I povolené ExpressRoute premium?

Hello následující tabulky popisují hello ExpressRoute omezení a hello počet virtuálních sítí na jeden okruh ExpressRoute:

[!INCLUDE [ExpressRoute limits](../../includes/expressroute-limits.md)]

### <a name="how-do-i-enable-expressroute-premium"></a>Povolení ExpressRoute premium

Funkce ExpressRoute premium můžete povolit, pokud je povolena funkce hello a může být vypnut aktualizací hello stav okruhu. Povolit v okamžiku vytvoření okruhu ExpressRoute premium, nebo můžete volat hello rozhraní API REST nebo rutiny prostředí PowerShell.

### <a name="how-do-i-disable-expressroute-premium"></a>Jakým způsobem vypnout ExpressRoute premium?

ExpressRoute premium můžete zakázat voláním hello rozhraní API REST nebo rutiny prostředí PowerShell. Musí se ujistěte, že máte škálovat vaše připojení musí toomeet hello výchozí omezení dříve, než zakážete ExpressRoute premium. Pokud vaše využití škáluje nad rámec hello výchozí omezení, hello požadavek toodisable ExpressRoute premium se nezdaří.

### <a name="can-i-pick-and-choose-hello-features-i-want-from-hello-premium-feature-set"></a>Může I vybrat hello funkce, která má být z sada funkcí premium hello?

Ne. Nelze vybrat hello funkce. Jsme povolení všech funkcí po zapnutí ExpressRoute premium.

### <a name="how-much-does-expressroute-premium-cost"></a>Kolik stojí ExpressRoute premium?

Odkazovat příliš[podrobnosti o cenách](https://azure.microsoft.com/pricing/details/expressroute/) náklady.

### <a name="do-i-pay-for-expressroute-premium-in-addition-toostandard-expressroute-charges"></a>Platím za ExpressRoute premium kromě toostandard poplatky ExpressRoute?

Ano. ExpressRoute premium poplatky za nad poplatky okruh ExpressRoute a poplatky, které vyžadují hello poskytovatele připojení.

## <a name="expressroute-for-office-365-and-dynamics-365"></a>ExpressRoute pro Office 365 a Dynamics 365

[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

### <a name="how-do-i-create-an-expressroute-circuit-tooconnect-toooffice-365-services-and-dynamics-365"></a>Vytvoření tooconnect okruh ExpressRoute tooOffice 365 služby a Dynamics 365?

1. Zkontrolujte hello [stránky požadavky ExpressRoute](expressroute-prerequisites.md) toomake, že splňujete požadavky hello.
2. tooensure vyžadující připojení k jsou splněny, projděte si seznam hello poskytovatelé a umístění v hello [ExpressRoute partnery a umístění](expressroute-locations.md) článku.
3. Plánování požadavků na kapacitu kontrolou [plánování sítě a optimalizace výkonu pro Office 365](http://aka.ms/tune/).
4. Postupujte podle kroků hello uvedených v tooset pracovních hello připojením [zřizování okruhů a stavy okruhů ve workflowech ExpressRoute](expressroute-workflows.md).

> [!IMPORTANT]
> Ujistěte se, že je povoleno doplněk ExpressRoute premium při konfiguraci služby tooOffice 365 připojení a Dynamics 365.
> 
> 

### <a name="do-i-need-tooenable-azure-public-peering-tooconnect-toooffice-365-services-and-dynamics-365"></a>Je nutné tooenable Azure veřejný partnerský vztah tooconnect tooOffice 365 služeb a Dynamics 365?

Ne, potřebujete jenom tooenable Peering společnosti Microsoft. Ověřování provozu tooAzure AD se budou odesílat prostřednictvím Peering společnosti Microsoft. 

### <a name="can-my-existing-expressroute-circuits-support-connectivity-toooffice-365-services-and-dynamics-365"></a>Můj stávající okruhy ExpressRoute může podporovat 365 služeb připojení k tooOffice a Dynamics 365?

Ano. Vaše stávající okruh ExpressRoute může být nakonfigurované toosupport připojení tooOffice 365 služby. Ujistěte se, zda máte dostatečná kapacita tooconnect tooOffice 365 služby a že je povoleno doplněk premium. [Plánování sítě a optimalizace výkonu pro Office 365](http://aka.ms/tune/) musí vám pomůže s plánováním připojení. Další informace naleznete v [vytvoření a úprava okruhu ExpressRoute](expressroute-howto-circuit-classic.md).

### <a name="what-office-365-services-can-be-accessed-over-an-expressroute-connection"></a>Jaké Office 365 služby je přístupná přes připojení ExpressRoute?

Odkazovat příliš[Office 365 adresy URL a rozsahy IP adres](http://aka.ms/o365endpoints) stránky pro aktuální seznam služeb podporovaných přes ExpressRoute.

### <a name="how-much-does-expressroute-for-office-365-services-and-dynamics-365-cost"></a>Kolik podporuje ExpressRoute pro služby Office 365 a náklady na Dynamics 365?

Služby Office 365 a Dynamics 365 vyžadují premium rozšíření toobe povolena. V tématu hello [stránce s podrobnostmi o cenách](https://azure.microsoft.com/pricing/details/expressroute/) pro náklady.

### <a name="what-regions-is-expressroute-for-office-365-supported-in"></a>Jaké oblasti je ExpressRoute pro Office 365 v podporovaná?

V tématu [ExpressRoute partnery a umístění](expressroute-locations.md) informace.

### <a name="can-i-access-office-365-over-hello-internet-even-if-expressroute-was-configured-for-my-organization"></a>Můžete získat přístup k Office 365 přes hello Internet, i v případě, že ExpressRoute byla nakonfigurována pro Moje organizace?

Ano. Koncové body Office 365 služby jsou dostupné prostřednictvím hello Internet, i když ExpressRoute byla nakonfigurována pro vaší sítě. Pokud jste v umístění, které je nakonfigurované tooconnect tooOffice 365 služby prostřednictvím ExpressRoute, můžete se připojit prostřednictvím ExpressRoute.

### <a name="can-i-access-office-365-us-government-community-gcc-services-over-an-azure-us-government-expressroute-circuit"></a>Můžete přístup služeb Office 365 US Government komunity (RSZ) přes US Government okruhu Azure ExpressRoute?

Ano. Koncové body služby Office 365 RSZ jsou dostupné prostřednictvím hello Azure US Government ExpressRoute. Však můžete první nutné tooopen lístek podpory na hello předpony hello Azure portálu tooprovide tooadvertise tooMicrosoft, který chcete. Vaše připojení tooOffice 365 RSZ služby bude vytvořeno po vyřešení lístku podpory hello. 

### <a name="can-dynamics-365-for-operations-formerly-known-as-dynamics-ax-online-be-accessed-over-an-expressroute-connection"></a>Dynamics 365 pro operace (dříve označované jako Dynamics AX Online) jsou přístupné přes připojení ExpressRoute?

Ano. [Dynamics 365 pro operace](https://www.microsoft.com/dynamics365/operations) je hostovaná v Azure. Můžete povolit veřejný partnerský vztah Azure na vaše tooit tooconnect okruhu ExpressRoute.

## <a name="route-filters-for-microsoft-peering"></a>Filtry tras pro partnerský vztah Microsoftu

### <a name="i-am-turning-on-microsoft-peering-for-hello-first-time-what-routes-will-i-see"></a>Mě zapnete partnerský vztah Microsoftu pro hello poprvé, jaké postupy je se zobrazí?

Neuvidíte žádné trasy. Máte tooattach trasy filtru tooyour okruhu toostart předponu oznámení o inzerovaném programu. Pokyny najdete v tématu [filtry konfigurace směrování pro partnerský vztah Microsoftu](how-to-routefilter-powershell.md).

### <a name="i-turned-on-microsoft-peering-and-now-i-am-trying-tooselect-exchange-online-but-it-is-giving-me-an-error-that-i-am-not-authorized-toodo-it"></a>I zapnutý partnerského vztahu Microsoftu a teď jsem snažíme tooselect Exchange Online, ale se mi je poskytnutí chybu, nejsem autorizovaný toodo ho.

Pokud používáte filtry tras, všechny zákazníka můžete zapnout partnerského vztahu Microsoftu. Pro využívání služeb Office 365, přesto však tooget oprávnění v rámci služeb Office 365.

### <a name="do-i-need-tooget-authorization-for-turning-on-dynamics-365-over-microsoft-peering"></a>Potřebuji tooget autorizace pro zapnutí Dynamics 365 prostřednictvím partnerského vztahu Microsoftu?

Ne, není nutné autorizace pro Dynamics 365. Můžete vytvořit pravidlo a vyberte Dynamics 365 community bez autorizace.

### <a name="i-already-have-microsoft-peering-how-can-i-take-advantage-of-route-filters"></a>Už mám partnerský vztah Microsoftu, jak můžete využít výhod filtry tras?

Můžete vytvořit filtr trasy, vyberte hello služeb, které chcete toouse a připojte hello partnerského vztahu Microsoftu tooyour filtru. Pokyny najdete v tématu [filtry konfigurace směrování pro partnerský vztah Microsoftu](how-to-routefilter-powershell.md).

### <a name="i-have-microsoft-peering-at-one-location-now-i-am-trying-tooenable-it-at-another-location-and-i-am-not-seeing-any-prefixes"></a>Je nutné Microsoft partnerský vztah na jednom místě, teď pokouším tooenable ho na jiné umístění a nejsou zobrazeny všechny předpony.

* Partnerského vztahu Microsoftu okruhy ExpressRoute, které byly nakonfigurovány předchozí tooAugust 1, 2017 budou mít všechny služby předpony inzerované prostřednictvím partnerského vztahu Microsoftu, i když nejsou definovány filtry tras.

* Okruhy ExpressRoute, které jsou nakonfigurované na nebo po 1 srpen 2017 partnerského vztahu Microsoftu nebude mít všechny předpony inzerované, dokud nebude připojené filtr trasy toohello okruh. Ve výchozím nastavení se zobrazí bez předpony.
