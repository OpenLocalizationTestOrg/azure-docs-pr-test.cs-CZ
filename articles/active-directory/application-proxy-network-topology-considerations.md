---
title: "aspekty topologie aaaNetwork při použití aplikace Proxy Azure Active Directory | Microsoft Docs"
description: "Popisuje aspekty topologie sítě, při použití Azure AD Application Proxy."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 9b8cdd2196efeb92a74e44dde6511f7d3091a968
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="network-topology-considerations-when-using-azure-active-directory-application-proxy"></a>Aspekty topologie sítě při použití aplikace Proxy Azure Active Directory

Tento článek vysvětluje aspekty topologie sítě, při použití aplikace Proxy Azure Active Directory (Azure AD) pro publikování a vzdálený přístup k vaší aplikace.

## <a name="traffic-flow"></a>Tok přenosů dat

Při publikování aplikace prostřednictvím proxy aplikace služby Azure AD, prochází provoz z aplikace toohello uživatelé hello tři připojení:

1. Hello se uživatel připojuje veřejný koncový bod aplikace toohello Azure AD Application Proxy služby v Azure
2. Hello Proxy aplikace služby připojí toohello konektor Proxy aplikace
3. konektor Proxy aplikace Hello připojí toohello cílové aplikace

![Diagram zobrazující přenosy z aplikace tootarget uživatele](./media/application-proxy-network-topologies/application-proxy-three-hops.png)

## <a name="tenant-location-and-application-proxy-service"></a>Umístění klienta a Proxy aplikace služby

Při registraci pro klienta služby Azure AD hello oblasti klienta služby je určen podle země hello, které určíte. Když povolíte Proxy aplikace, instance služby hello Proxy aplikace vašeho klienta jsou zvolené nebo vytvořené v hello stejné oblasti jako klient Azure AD nebo nejbližší oblast tooit hello.

Pokud je klient Azure AD oblast hello Evropské unie (EU), všechny konektory Proxy aplikací pomocí instance služby v datových centrech Azure v hello Evropa. Při publikování aplikace přístup vašich uživatelů jejich provoz prochází hello instance služby Proxy aplikace v tomto umístění.

## <a name="considerations-for-reducing-latency"></a>Důležité informace týkající se sníží se latence

Všechna řešení pro proxy představovat latence na připojení k síti. Bez ohledu na to, které proxy server nebo řešení sítě VPN zvolíte jako řešení pro vzdálený přístup vždy obsahuje sadu servery umožňující připojení tooinside hello vaší podnikové síti.

Organizace obvykle obsahovat koncové body serveru v jejich hraniční síti. S Azure AD Application Proxy ale přenosy dat pomocí služby proxy hello v cloudu hello při hello konektory jsou umístěné ve vaší podnikové síti. Je potřeba žádné hraniční síť.

Hello další oddíly obsahují další návrhy toohelp snížit latenci ještě víc. 

### <a name="connector-placement"></a>Umístění konektoru

Proxy aplikací zvolí hello umístění instancí pro vás, na základě vašeho umístění klienta. Však můžete získat toodecide kde tooinstall hello konektoru, která poskytuje hello power toodefine hello latence charakteristiky síťových přenosů.

Při nastavování hello Proxy aplikace služby, požádejte hello následující otázky:

* Kde je umístěná aplikace hello?
* Kde jsou většina uživatelů, kteří přístup k aplikaci hello umístěné?
* Instance Proxy aplikace hello umístění?
* Už máte datových centrech vyhrazenou síť připojení tooAzure nastavení, jako je Azure ExpressRoute nebo podobné VPN?

konektor Hello má toocommunicate s Azure a aplikací (kroky 2 a 3 v diagram toku dat hello), takže hello umístění hello konektor ovlivňuje hello latence tyto dvě připojení. Při vyhodnocení umístění hello hello konektoru, mějte na paměti hello následující body:

* Pokud chcete toouse omezené delegování protokolu Kerberos (použitím KCD) pro jednotné přihlašování, konektor hello musí datacentru tooa směrem pohledu. Kromě toho hello serveru konektoru musí toobe připojený k doméně.  
* Pokud máte pochybnosti, nainstalujte aplikace blíže toohello hello konektor.

### <a name="general-approach-toominimize-latency"></a>Obecný přístup toominimize latence

Můžete-li minimalizovat hello latence hello začátku do konce provozu, optimalizace každé síťové připojení. Každé připojení možné ji optimalizovat podle:

* Sníží hello vzdálenost mezi hello dva elementy end hello směrování.
* Výběr správného síťového tootraverse hello. Například procházení privátní síti spíše než hello veřejného Internetu může být rychlejší, z důvodu toodedicated odkazy.

Pokud máte vyhrazené sítě VPN nebo ExpressRoute propojení mezi Azure a k podnikové síti, může být vhodné toouse který.

## <a name="focus-your-optimization-strategy"></a>Zaměřit strategie optimalizace

Velmi malé, abyste provedli toocontrol hello připojení mezi uživateli a hello Proxy aplikace služby. Uživatelé mohou přistupovat k vaší aplikace z domácí sítě, v kavárně nebo jiné země. Místo toho můžete optimalizovat hello připojení z hello Proxy aplikace služby toohello Proxy aplikace konektory toohello aplikací. Zvažte využití hello následující vzory ve vašem prostředí.

### <a name="pattern-1-put-hello-connector-close-toohello-application"></a>Vzor 1: Aplikace zavřít toohello konektor hello Put

Aplikace cílové místo hello konektor zavřít toohello v síti zákazníka hello. Tato konfigurace minimalizuje krok 3 v diagramu topografie hello, protože konektor hello a aplikace jsou zavřít. 

Pokud vaše konektor potřebuje směrem pohledu toohello řadiče domény, je tento vzor výhodné. Většina našich zákazníků použít tento vzor, protože je vhodný pro většinu scénářů. Tento vzor může být spojen s vzor 2 toooptimize provoz mezi službou hello a konektor hello.

### <a name="pattern-2-take-advantage-of-expressroute-with-public-peering"></a>Vzor 2: Využít výhod ExpressRoute s veřejného partnerského vztahu

Pokud máte nastavit veřejného partnerského vztahu ExpressRoute, můžete pro přenosy mezi Proxy aplikací a konektor hello hello rychlejší připojení ExpressRoute. konektor Hello je stále v síti, toohello zavřít aplikaci.

### <a name="pattern-3-take-advantage-of-expressroute-with-private-peering"></a>Vzor 3: Využít výhod ExpressRoute s soukromého partnerského vztahu

Pokud máte vyhrazené sítě VPN nebo ExpressRoute nastavit s soukromého partnerského vztahu mezi Azure a k podnikové síti, existuje další možnost. V této konfiguraci se obvykle považuje hello virtuální sítě v Azure jako rozšíření hello podnikové sítě. Proto můžete nainstalovat konektor hello v hello datové centrum Azure a stále splňují požadavky s nízkou latencí hello hello připojení konektoru aplikace.

Latence nejsou ohrožená, protože provoz je předávaných přes vyhrazené připojení. Zlepšení latence služby konektoru Proxy aplikace můžete také získat, protože konektor hello nainstalovaný v zavřít tooyour datové centrum Azure umístění klienta Azure AD.

![Diagram zobrazující konektoru nainstalovány v datovém centru Azure](./media/application-proxy-network-topologies/application-proxy-expressroute-private.png)

### <a name="other-approaches"></a>Jiné postupy

I když hello cílem tohoto článku je konektor umístění, můžete také změnit umístění hello hello aplikace tooget lepší latence charakteristiky.

Organizace stále, jsou přesunutí jejich sítě do hostovaného prostředí. Díky tomu mohou tooplace své aplikace v hostovaném prostředí, který je také součástí své podnikové síti a přesto být v rámci domény hello. Vzory hello popsané v předchozích částech hello v takovém případě může být použité toohello nové umístění aplikace. Pokud zvažujete tuto možnost, přečtěte si téma [Azure AD Domain Services](../active-directory-domain-services/active-directory-ds-overview.md).

Kromě toho zvažte uspořádání vaší konektorů pomocí [konektor skupiny](active-directory-application-proxy-connectors.md) tootarget aplikace, které jsou v různých umístěních a sítě. 

## <a name="common-use-cases"></a>Běžné případy použití

V této části jsme provede několik běžných scénářů. Předpokládejme, že hello klienta Azure AD (a proto proxy služby koncového bodu) se nachází v hello USA (US). Hello aspekty, které jsou popsané v těchto případech použití platí také tooother oblasti kolem hello zeměkouli.

Pro tyto scénáře jsme volání každé připojení "směrování" a číslo je snazší diskusi:

- **Směrování 1**: uživatel toohello Proxy aplikace služby
- **Směrování 2**: konektoru Proxy aplikace toohello Proxy aplikace služby
- **Směrování 3**: Proxy aplikace konektor toohello cílové aplikace 

### <a name="use-case-1"></a>Případ použití 1

**Scénář:** hello aplikace je v síti organizace v hello nám, s uživateli v hello stejné oblasti. Žádné ExpressRoute nebo VPN existuje mezi hello datové centrum Azure a hello podnikové síti.

**Doporučení:** vzor postupujte podle kroků 1, popsané v předchozí části hello. Pro zlepšení latence vezměte v úvahu pomocí služby ExpressRoute, v případě potřeby.

Toto je jednoduchá vzor. Můžete tak, že konektor hello téměř aplikace hello optimalizovat směrování 3. Toto je také přirozené volbou, protože hello konektor je obvykle nainstalován s směrem pohledu toohello aplikace a toohello operací datového centra tooperform použitím KCD.

![Diagram znázorňující, že uživatelé, proxy, konektor a aplikace jsou v hello nám](./media/application-proxy-network-topologies/application-proxy-pattern1.png)

### <a name="use-case-2"></a>Případ použití 2

**Scénář:** hello aplikace je v síti organizace v hello nám, s uživateli šíření globálně. Žádné ExpressRoute nebo VPN existuje mezi hello datové centrum Azure a hello podnikové síti.

**Doporučení:** vzor postupujte podle kroků 1, popsané v předchozí části hello. 

Znovu hello běžné vzor je toooptimize směrování 3, kde umístit hello konektor téměř aplikace hello. Směrování 3 není obvykle levnější, pokud je přímo v aplikaci hello stejné oblasti. Směrování 1 však může být dražší, v závislosti na tom, kde je uživatel hello, protože uživatelé napříč hello, world musí získat přístup k instanci Proxy aplikace hello v hello USA. Je vhodné poznamenat, že řešení proxy serveru má podobnou charakteristikou týkající se uživatelů se šíření globálně.

![Diagram zobrazující, že uživatelé jsou rozloženy globálně, ale hello proxy, konektor a aplikace jsou v hello USA](./media/application-proxy-network-topologies/application-proxy-pattern2.png)

### <a name="use-case-3"></a>Případ použití 3

**Scénář:** hello aplikace je v síti organizace v hello nás. Existuje ExpressRoute s veřejný partnerský vztah mezi Azure a hello podnikové síti.

**Doporučení:** postupujte podle vzorů 1 a 2, které jsou popsané v předchozí části hello.

Nejprve umístíte hello konektor co možná toohello aplikace. Potom hello systém automaticky používá ExpressRoute pro směrování 2. 

Pokud odkaz ExpressRoute hello používá veřejný partnerský vztah, hello mezi hello proxy a konektor hello přenosy přes tento odkaz. Směrování 2 optimalizovala latence.

![Diagram zobrazující ExpressRoute mezi hello proxy a konektor](./media/application-proxy-network-topologies/application-proxy-pattern3.png)

### <a name="use-case-4"></a>Případ použití 4

**Scénář:** hello aplikace je v síti organizace v hello nás. Existuje ExpressRoute s soukromého partnerského vztahu mezi Azure a hello podnikové síti.

**Doporučení:** postupujte podle vzoru 3, popsané v předchozí části hello.

Místní konektor hello v hello datové centrum Azure, který je připojený toohello podnikové síti prostřednictvím soukromého partnerského vztahu ExpressRoute. 

konektor Hello mohou být umístěny v hello datového centra Azure. Vzhledem k tomu, že konektor hello má stále směrem pohledu toohello aplikace a datacenter hello prostřednictvím privátní sítě hello, zůstane optimalizované směrování 3. Kromě toho směrování 2 je optimalizovaná Další.

![Diagram zobrazující hello konektor v datovém centru Azure a ExpressRoute mezi hello konektoru a aplikace](./media/application-proxy-network-topologies/application-proxy-pattern4.png)

### <a name="use-case-5"></a>Případ použití 5

**Scénář:** hello aplikace je v síti organizace v instanci služby Proxy aplikace hello a většina uživatelů v hello hello Evropa, USA.

**Doporučení:** místní konektor hello téměř aplikace hello. Protože USA uživatelé přistupují k instanci Proxy aplikace, která se stane toobe v hello stejné oblasti směrování 1 není příliš nákladné. 3 směrování je optimalizované. Zvažte použití směrování ExpressRoute toooptimize 2. 

![Diagram zobrazující uživatelů a proxy serveru v USA, hello s hello konektoru a aplikace v hello Evropa](./media/application-proxy-network-topologies/application-proxy-pattern5b.png)

Můžete také použít jeden další variant v této situaci. Pokud většina uživatelů v organizaci hello v hello nám, pak pravděpodobné že síti rozšiřuje toohello nám také. Umístit hello konektor hello USA a používání aplikace hello vyhrazené interní podnikové síti řádku toohello v hello Evropa. Tento způsob směrování 2 a 3 jsou optimalizované.

![Diagram zobrazující proxy, uživatelů a konektor v USA, aplikace v hello Evropa hello](./media/application-proxy-network-topologies/application-proxy-pattern5c.png)

## <a name="next-steps"></a>Další kroky

- [Povolení Proxy aplikace](active-directory-application-proxy-enable.md)
- [Povolení jednoduchého přihlášení](active-directory-application-proxy-sso-using-kcd.md)
- [Povolení podmíněného přístupu](active-directory-application-proxy-conditional-access.md)
- [Řešení problémů, které máte s pomocí Proxy aplikace](active-directory-application-proxy-troubleshoot.md)
