---
title: aaaWhat je Traffic Manageru | Microsoft Docs
description: "Tento článek vám pomůže pochopit, co je Traffic Manager, a zda je hello správné provoz směrování volbu pro vaši aplikaci"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 75d5ff9a-f4b9-4b05-af32-700e7bdfea5a
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/15/2017
ms.author: kumud
ms.openlocfilehash: 8e63ed11cdcdc03ae9cd28f88f0d1f9dc2cd44ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-traffic-manager"></a>Přehled Traffic Manageru

Microsoft Azure Traffic Manager umožňuje toocontrol hello distribuce provozu generovaného uživateli pro koncové body služby v různých datových centrech. Koncové body služby, které jsou podporovány nástrojem Traffic Manager zahrnují virtuálních počítačích Azure, webové aplikace a cloudových služeb. Službu Traffic Manager můžete používat také s externími koncovými body mimo Azure.

Traffic Manager používá hello systému DNS (Domain Name) toodirect klienta požadavky toohello nejvhodnější koncový bod na základě metodu směrování provozu a stavu hello hello koncových bodů. Traffic Manager poskytuje řadu [metody směrování provozu](traffic-manager-routing-methods.md) a [koncový bod možnosti monitorování](traffic-manager-monitoring.md) jinou aplikaci toosuit potřeb a modely automatické převzetí služeb při selhání. Správce provozu je odolný toofailure, včetně hello selhání celé oblasti Azure.

## <a name="traffic-manager-benefits"></a>Výhody Traffic Manageru

Traffic Manager vám pomůžou:

* **Zvýšit dostupnost důležitých aplikací**

    Traffic Manager poskytuje vysokou dostupnost pro vaše aplikace prostřednictvím sledování koncových bodů a zajištění automatické převzetí služeb při selhání, kdy koncový bod přestane fungovat.

* **Zvýšení rychlosti odezvy pro vysoce výkonné aplikace**

    Azure umožňuje toorun cloudové služby nebo webů v datových centrech nachází kolem hello, world. Správce provozu zlepšuje odezvu aplikací přesměrováním provoz toohello koncový bod s nejnižší latenci sítě hello pro klienta hello.

* **Provedení údržby služby bez výpadků**

    Můžete provádět operace plánované údržby na vaše aplikace bez výpadků. Traffic Manager přesměruje tooalternative provoz koncovými body, když probíhá hello údržby.

* **Kombinovat místní a cloudové aplikace**

    Traffic Manager podporuje externí, mimo Azure koncové body jeho povolení toobe použít s hybridní cloud a místní nasazení, včetně hello "shluků to-cloud," "migrovat do cloudu," a "převzetí služeb při selhání cloud" scénáře.

* **Distribuce přenosů pro složitých nasazení**

    Pomocí [vnořené profily Traffic Manageru](traffic-manager-nested-profiles.md), metody směrování provozu může být kombinované toocreate sofistikované a flexibilní pravidla toosupport hello musí rozsáhlejších, složitějších nasazení.

## <a name="how-traffic-manager-works"></a>Jak funguje Traffic Manager

Azure Traffic Manager umožňuje toocontrol hello distribuce přenosů mezi koncových bodů vaší aplikace. Koncový bod je všechny internetové služby hostované uvnitř nebo mimo Azure.

Správce provozu přináší dvě klíčové výhody:

1. Distribuce přenosů podle tooone několik [metody směrování provozu](traffic-manager-routing-methods.md)
2. [Nepřetržité monitorování stavu koncový bod](traffic-manager-monitoring.md) a automatické převzetí služeb při selhání koncových bodů

Když se klient pokusí tooconnect tooa služby, se musí nejprve přeložit název DNS hello hello služby tooan IP adresy. Klient Hello se pak připojí toothat IP adresu tooaccess hello služby.

**Nejdůležitější toounderstand bodu Hello je, že Traffic Manager funguje na úrovni DNS hello.**  Koncové body na základě pravidel hello hello metody směrování provozu Traffic Manager používá DNS toodirect klienti toospecific služby. Koncový bod toohello vybrané se klienti připojují **přímo**. Traffic Manager není serveru proxy nebo brána. Správce provozu nezná hello provoz předávání mezi hello klientem a službou hello.

### <a name="traffic-manager-example"></a>Příklad Traffic Manageru

Společnosti Contoso Corp si vytvořili nový portál pro partnery. Hello adresa URL pro tento portál je https://partners.contoso.com/login.aspx. aplikace Hello je hostována do tří oblastí ve službě Azure. tooimprove dostupnosti a maximalizovat výkon globální, používají Traffic Manager toodistribute klienta provoz toohello nejbližší dostupný koncový bod.

tooachieve tato konfigurace po dokončení hello následující kroky:

1. Nasaďte tři instance své služby. názvy DNS Hello tato nasazení jsou 'contoso-us.cloudapp .net', 'contoso-eu.cloudapp .net nebo 'contoso-asia.cloudapp .net'.
2. Vytvoření profilu Traffic Manageru, s názvem "contoso.trafficmanager.net a nakonfigurujte ho toouse hello směrování provozu, výkon, metoda napříč koncovými body hello tři.
* Konfigurace jejich název domény jednoduché, "partners.contoso.com", toopoint too'contoso.trafficmanager.net', pomocí záznam DNS CNAME.

![Konfigurace služby DNS Traffic Manageru][1]

> [!NOTE]
> Pokud používáte jednoduché doménu Azure Traffic Manager, musíte použít CNAME toopoint vaší jednoduché domény název tooyour název domény Traffic Manageru. Standardech DNS vám nepovolují toocreate záznam CNAME v hello 'vrcholu' (nebo kořenové) k doméně. Proto nelze vytvořit záznam CNAME pro "contoso.com" (někdy se označuje jako "holé" domény). Můžete vytvořit pouze záznam CNAME pro doménu v části "contoso.com", třeba "www.contoso.com". toowork obejít toto omezení, doporučujeme používat jednoduché požadavky toodirect přesměrování protokolu HTTP pro "contoso.com" tooan alternativní název třeba "www.contoso.com".

### <a name="how-clients-connect-using-traffic-manager"></a>Způsob připojení klientů pomocí Traffic Manager

Pokračováním z předchozího příkladu hello, když klient požádá o https://partners.contoso.com/login.aspx stránku hello hello klienta provede následující kroky tooresolve hello DNS název hello a naváže připojení:

![Navázání připojení pomocí Traffic Manager][2]

1. Hello klient odešle tooits nakonfigurované rekurzivního dotazu DNS DNS služby tooresolve hello název "partners.contoso.com". Služba DNS rekurzivní, někdy označuje jako "místní DNS, služby, není hostitelem DNS domény přímo. Místo toho klienta hello off-loads hello pracovní různé autoritativních DNS z kontaktování hello services přes Internet potřeby tooresolve hello název DNS.
2. název DNS hello tooresolve, služba DNS rekurzivní hello vyhledá hello názvové servery pro doménu "contoso.com" hello. Potom kontaktuje těchto název servery toorequest hello "partners.contoso.com" záznam DNS. servery DNS contoso.com Hello vrátí hello záznam CNAME, který odkazuje toocontoso.trafficmanager.net.
3. V dalším kroku služba DNS rekurzivní hello vyhledá hello názvové servery pro doménu "trafficmanager.net" hello, které jsou poskytovány hello služba Azure Traffic Manager. Pak odešle žádost o hello 'contoso.trafficmanager.net' DNS záznamů toothose servery DNS.
4. Hello Traffic Manager názvové servery přijmout žádost o hello. Vybírá koncový bod na základě:

    - Stav Hello nakonfigurovat každý koncový bod (nebudou zobrazeny zakázané koncových bodů)
    - kontroluje, aktuální stav Hello každý koncový bod určeného hello Traffic Manager stavu. Další informace najdete v tématu [Traffic Manager koncového bodu monitorování](traffic-manager-monitoring.md).
    - Hello vybrali jako metodu směrování provozu. Další informace najdete v tématu [metody směrování Traffic Manager](traffic-manager-routing-methods.md).

5. koncový bod Hello vybrali se vrátí jako jiný záznam DNS CNAME. V takovém případě se dejte nám Předpokládejme, že je vrácen contoso us.cloudapp.net.
6. V dalším kroku služba DNS rekurzivní hello vyhledá hello názvové servery pro doménu "cloudapp.net" hello. Kontaktuje těchto název servery toorequest hello "contoso-us.cloudapp .net, záznam DNS. Záznam DNS "A", obsahující hello IP adresa koncového bodu služeb na základě USA hello je vrácen.
7. Služba DNS rekurzivní Hello konsoliduje hello výsledky a vrátí jednoho klienta toohello odpovědi DNS.
8. Hello klienta obdrží výsledky hello DNS, připojí se toohello danou IP adresu. Hello klient připojí koncový bod služby toohello aplikace přímo, nejsou prostřednictvím Správce provozu. Vzhledem k tomu, že je koncový bod HTTPS, provede hello nezbytné SSL/TLS handshake hello klienta a pak provede požadavek HTTP GET pro hello "/ login.aspx' stránky.

Služba DNS rekurzivní Hello ukládá do mezipaměti hello odpovědí DNS, které obdrží. překladač služby DNS Hello hello klientského zařízení také ukládá do mezipaměti hello výsledek. Ukládání do mezipaměti umožňuje následné toobe dotazy DNS odpovědi rychleji pomocí dat z mezipaměti hello spíše než dotazování další názvové servery. Doba trvání Hello hello mezipaměti je určen vlastností hello 'time-to-live' (TTL) každého záznamu DNS. Kratší hodnoty za následek rychlejší vypršení platnosti mezipaměti a proto další odezvy toohello Traffic Manager názvové servery. Delší hodnoty znamená, že může trvat déle toodirect provoz směrem od koncový bod se nezdařilo. Traffic Manager umožňuje tooconfigure hello TTL používá v toobe odpovědí DNS Traffic Manageru v rozsahu od 0 sekund až na 2 147 483 647 sekund (hello maximální rozsah kompatibilní s [definicí RFC 1035](https://www.ietf.org/rfc/rfc1035.txt)), umožňuje toochoose hello hodnota který vyrovnává nejlépe hello potřebám vaší aplikace.

## <a name="pricing"></a>Ceny

Informace o cenách najdete v části [Traffic Manager cenová](https://azure.microsoft.com/pricing/details/traffic-manager/).

## <a name="faq"></a>Nejčastější dotazy

Nejčastější dotazy o Traffic Manager, najdete v části [Traffic Manager nejčastější dotazy](traffic-manager-FAQs.md)

## <a name="next-steps"></a>Další kroky

Další informace o Traffic Manager [koncového bodu monitorování a automatické převzetí služeb při selhání](traffic-manager-monitoring.md).

Další informace o Traffic Manager [metodách směrování provozu](traffic-manager-routing-methods.md).

Další informace o některých hello Další klíč [sítě možnosti](../networking/networking-overview.md) Azure.

<!--Image references-->
[1]: ./media/traffic-manager-how-traffic-manager-works/dns-configuration.png
[2]: ./media/traffic-manager-how-traffic-manager-works/flow.png

