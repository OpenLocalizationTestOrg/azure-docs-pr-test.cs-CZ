---
title: "směrování aaaAsymmetric | Microsoft Docs"
description: "Tento článek vás provede hello problémy, se kterými může zákazník čelí s asymetrické směrování v síti, která má více cílové tooa odkazy."
documentationcenter: na
services: expressroute
author: osamazia
manager: carmonm
editor: 
ms.assetid: a754bff9-95c9-44b5-9796-377fc21e8322
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: osamam
ms.openlocfilehash: 01a16242437a3674dcfe27b074911a829a6c1abd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="asymmetric-routing-with-multiple-network-paths"></a>Asymetrické směrování s několika síťovými cestami
Tento článek vysvětluje, jak může dopředný a zpětný síťový provoz využívat různé trasy, pokud je mezi zdrojem a cílem v síti k dispozici více cest.

Jeho důležité toounderstand dvěma konceptů toounderstand asymetrické směrování. Jeden je hello účinku více síťových cest. Hello jiných je, jak zařízení, jako je brána firewall, udržování stavu. Pro tyto typy zařízení se používá označení stavová zařízení. Kombinace tyto dva faktory vytvoří scénáře, ve které síti stavová zařízení vyřadit provoz protože hello stavová zařízení nebylo zjistit, jestli provoz pochází s samotné zařízení hello.

## <a name="multiple-network-paths"></a>Několik síťových cest
Když je podniková síť má jenom jeden toohello odkaz, který se přenáší Internet prostřednictvím jejich poskytovatele internetových služeb, tooand všechny přenosy z Internetu hello hello stejnou cestu. Často společností zakoupit více okruhů jako redundantní cesty, tooimprove provozu sítě. Pokud k tomu dojde, jeho možné, že provozu, který prochází mimo síť hello, toohello Internetu, prochází jedním odkazem a hello návratový provoz prochází různé odkaz. To se běžně označuje jako asymetrické směrování. Zpětné síťový provoz v asymetrické směrování, trvá jinou cestu z původní toku hello.

![Síť s více cestami](./media/expressroute-asymmetric-routing/AsymmetricRouting3.png)

I když dojde k především na hello Internet, taky asymetrické směrování platí tooother kombinace více cest. Se vztahuje, například tooan internetové trasy a privátní cestu, která přejděte toohello stejný cíl a toomultiple privátní cesty, které přejděte toohello stejný cíl.

Každému směrovači na způsob hello ze zdroje toodestination vypočítá hello nejlepší cesta tooreach cíl. Hello směrovače určení nejlepší možný cesta je založeno na dvě hlavní faktory:

* Směrování mezi externími sítěmi je založené na směrovacím protokolu BGP (Border Gateway Protocol). Protokol BGP trvá oznámení o inzerovaném programu z okolí a spustí je pomocí několika kroků toodetermine hello nejlepší cesta toohello určený cíl. Nejlepší cestu hello je uložený v jeho směrovací tabulky.
* Délka Hello masku podsítě spojenou s trasu vliv směrování cesty. Pokud směrovač přijme více oznámení o inzerovaném programu pro hello stejnou IP adresu, ale s masek jiné podsíti, směrovač hello upřednostní hello oznámení o inzerovaném programu s maskou podsítě déle, protože považuje za konkrétnější trasy.

## <a name="stateful-devices"></a>Stavová zařízení
Směrovače podívejte se na záhlaví IP hello paketu pro účely směrování. Podívejte se i hlubší v paketu hello některá zařízení. Tato zařízení zpravidla zkoumají hlavičky vrstvy 4 (protokol TCP nebo UDP), nebo dokonce vrstvy 7 (aplikační vrstva). Tyto typy zařízení patří buď mezi zařízení zabezpečení, nebo mezi zařízení optimalizace šířky pásma. 

Brána firewall je obvyklým příkladem stavového zařízení. Brána firewall povoluje nebo zakazuje toopass paketů přes jeho rozhraní na základě různých polí, jako je například protokol, port TCP/UDP a hlavičky adresy URL. Tato úroveň kontroly paketů zařadí se při velkém zatížení hello zařízení zpracování. výkon tooimprove brány firewall hello zkontroluje hello prvního paketu toku. Pokud umožňuje hello paketu tooproceed, zachová hello toku informací v tabulce jeho stav. Všechny následující pakety související toothis toku jsou povoleny, založené na první určení hello. Paketu, který je součástí existující toku může přicházejí na hello brány firewall. Pokud brána firewall hello žádné předchozí stav informace o tom, brány firewall hello zahodí hello paketů.

## <a name="asymmetric-routing-with-expressroute"></a>Asymetrické směrování s ExpressRoute
Jakmile se připojíte přes Azure ExpressRoute tooMicrosoft, síťové změny podobné výjimky:

* Máte několik tooMicrosoft odkazy. Jeden odkaz je stávajícího připojení k Internetu a hello jiných přes ExpressRoute. Některé tooMicrosoft provoz může projít hello Internet ale vraťte přes ExpressRoute, nebo naopak.
* Prostřednictvím ExpressRoute dostáváte mnohem konkrétnější IP adresy. Pro provoz z vaší sítě tooMicrosoft pro službám nabízeným přes ExpressRoute, je tedy směrovače vždy přednost ExpressRoute.

toounderstand hello vliv tyto dvě změny mít v síti, zvažte některé scénáře. Například máte pouze jeden okruh toohello Internetu a využívat všechny služby společnosti Microsoft prostřednictvím Internetu hello. Hello provoz z vaší sítě tooMicrosoft a zpět traverses hello stejné internetového odkazu a předává přes bránu firewall hello. zaznamenává brány firewall Hello hello toku tak, jak se zobrazí hello prvního paketu a vrátit pakety jsou povoleny, protože hello toku existuje v tabulce stavu hello.

![Asymetrické směrování s ExpressRoute](./media/expressroute-asymmetric-routing/AsymmetricRouting1.png)

Pak zapnete ExpressRoute a začnete využívat služby nabízené Microsoftem prostřednictvím ExpressRoute. Všechny služby společnosti Microsoft jsou spotřebováno přes hello Internetu. Nasadíte samostatné brána firewall vaší hraniční síti, který je připojený tooExpressRoute. Microsoft inzeruje konkrétnější předpony tooyour sítě prostřednictvím ExpressRoute pro určité služby. Směrovací infrastruktury zvolí ExpressRoute jako hello upřednostňované cestě pro tyto předpony. Pokud vaše veřejné IP adresy tooMicrosoft nejsou inzerování přes ExpressRoute, Microsoft komunikuje se veřejné IP adresy prostřednictvím hello Internetu. Předat dál provoz z vaší sítě tooMicrosoft používá ExpressRoute a zpětného provoz z Microsoft hello Internetu. Pokud brána firewall hello na hranici hello uvidí paket odezvy pro toku, který nebyl nalezen v tabulce stavu hello, zahodí hello návratový provoz.

Pokud si zvolíte toouse hello fond stejné překlad síťových adres (NAT) pro ExpressRoute a hello Internetu, se zobrazí podobné problémy s hello klienty v síti na soukromé IP adresy. Žádosti o služby, jako je Windows Update přejděte prostřednictvím hello Internet, protože IP adres pro tyto služby nejsou inzerované prostřednictvím ExpressRoute. Nicméně návratový provoz hello zpátky přes ExpressRoute. Pokud Microsoft přijme IP adresu s hello stejnou masku podsítě z hello Internet a ExpressRoute, dává přednost ExpressRoute přes hello Internetu. Pokud je brána firewall nebo jiné stavové zařízení, které je ve vaší hraniční sítě a čelí ExpressRoute nemá žádné předchozí informace o toku hello, zahodí pakety hello, které patří toothat toku.

## <a name="asymmetric-routing-solutions"></a>Řešení asymetrického směrování
Máte dvě základní možnosti toosolve hello problém asymetrické směrování. Jedna je prostřednictvím směrování a hello jiné, je použít na základě zdrojové NAT (SNAT).

### <a name="routing"></a>Směrování
Zajistěte, aby veřejné IP adresy inzerovaný tooappropriate širokopásmové sítě (WAN) odkazy. Například pokud chcete toouse hello Internetu pro ověřování provozu a ExpressRoute pro e-mailu provozu, by neměl inzerovat veřejné IP adresy služby Active Directory Federation Services (AD FS) přes ExpressRoute. Podobně, ujistěte se, není tooexpose na místní adresy tooIP serveru služby AD FS, které hello směrovač přijme přes ExpressRoute. Trasy přijatých prostřednictvím ExpressRoute jsou konkrétnější, takže provádění ExpressRoute hello upřednostňovaná cesta pro ověřování provozu tooMicrosoft. Tím je způsobeno asymetrické směrování.

Pokud chcete pro ověřování toouse ExpressRoute, ujistěte se, že jsou inzerování veřejné IP adresy služby AD FS přes ExpressRoute bez adres (NAT) Tímto způsobem, provoz, který pochází od společnosti Microsoft a přejde tooan místního serveru služby AD FS prochází přes ExpressRoute. Návratový provoz z tooMicrosoft zákazníka používá ExpressRoute, protože je upřednostňovaný trasy hello nad hello Internetu.

### <a name="source-based-nat"></a>Překlad adres na základě zdroje
Jiný způsob řešení problémů asymetrického směrování je prostřednictvím překladu adres na základě zdroje (SNAT). Například nebyly inzerovaný hello veřejnou IP adresu serveru Simple Mail Transfer Protocol (SMTP) místní přes ExpressRoute, protože hodláte toouse hello Internet pro tento typ komunikace. Požadavek, který pochází se společností Microsoft a pak přejde tooyour místního serveru SMTP prochází hello Internetu. Můžete překládat pomocí SNAT hello příchozí požadavek tooan interní IP adresu. Zpětné provoz ze serveru SMTP hello přejde toohello hraniční bráně firewall (který používáte pro NAT) místo prostřednictvím ExpressRoute. Návratový provoz Hello přejde zpět prostřednictvím hello Internetu.

![Konfigurace sítě s překladem adres na základě zdroje](./media/expressroute-asymmetric-routing/AsymmetricRouting2.png)

## <a name="asymmetric-routing-detection"></a>Detekce asymetrického směrování
Příkaz Traceroute je hello nejlepší způsob, jak toomake jistotu, že síťový provoz prochází přes hello očekávána cesta. Pokud očekáváte, provoz z vaší místní SMTP server tooMicrosoft tootake hello Internet cesty, hello očekává, že je příkaz traceroute z hello SMTP server tooOffice 365. výsledek Hello ověří, že provozu po skončení skutečně síti směrem k hello Internetu a nikoli k ExpressRoute.

