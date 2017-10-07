---
title: "Překlad adres (NAT) pro Azure ExpressRoute | Dokumentace Microsoftu"
description: "Tato stránka obsahuje podrobné požadavky pro konfiguraci a správu směrování pro okruhy ExpressRoute."
documentationcenter: na
services: expressroute
author: osamazia
manager: ganesr
editor: 
ms.assetid: eaaf0393-d384-4496-9a5c-328e94c262a7
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: osamam
ms.openlocfilehash: 7a8b760df90b545b5fbde2f614aef62dd3985bb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="nat-for-expressroute"></a>Překlad adres (NAT) pro ExpressRoute

cloudové služby tooMicrosoft tooconnect pomocí služby ExpressRoute, budete potřebovat tooset nahoru a spravovat směrování. Někteří poskytovatelé připojení nabízejí nastavení a správu směrování jako spravovanou službu. Pokud tuto službu nabízí, obraťte se na vaše toosee poskytovatele připojení. Pokud ne, je nutné splnit následující požadavky toohello. 

Odkazovat toohello [okruhy a domény směrování](expressroute-circuit-peerings.md) článku popis hello směrování relací, které je třeba toobe nastavit toofacilitate připojení.

> [!NOTE]
> Microsoft nepodporuje žádné protokoly redundance směrovačů (např. HSRP nebo VRRP) pro konfigurace s vysokou dostupností. V případě vysoké dostupnosti spoléháme na redundantní dvojici relací protokolu BGP na partnerský vztah.
> 
> 

## <a name="ip-addresses-used-for-peerings"></a>IP adresy sloužící pro partnerské vztahy

Je třeba tooreserve několik bloků IP adres tooconfigure směrování mezi vaší sítí a směrovači Microsoftu Enterprise edge (Msee). Tato část obsahuje seznam požadavků a popisuje hello pravidla o tom, jak tyto IP adresy musí být získat a použít.

### <a name="ip-addresses-used-for-azure-private-peering"></a>IP adresy sloužící pro soukromý partnerský vztah Azure

Můžete použít buď soukromé IP adresy nebo veřejné IP adresy tooconfigure hello partnerských vztahů. Hello rozsah adres použitý ke konfiguraci tras se nesmí překrývat s adresy rozsahů používají toocreate virtuálních sítí v Azure. 

* Pro rozhraní směrování musíte rezervovat podsíť /29 nebo dvě podsítě /30.
* Hello podsítě pro směrování může být buď soukromé IP adresy nebo veřejné IP adresy.
* Hello podsítě nesmí být v konfliktu s hello rozsahem vyhrazeným zákazníkem hello pro použití v cloudu Microsoft hello.
* Pokud se používá podsíť /29, rozdělí se na dvě podsítě /30. 
  * nejprve Hello/30 podsítě se použije pro primární propojení hello a hello druhý /30 podsítě se použije pro sekundární propojení hello.
  * Pro každé podsítě hello /30 musíte použít hello první IP adresu podsítě hello /30 na směrovači. Microsoft použije hello druhou IP adresu podsítě tooset hello /30 se relace protokolu BGP.
  * Musíte nastavit obě relace protokolu BGP pro naše [smlouva SLA o dostupnosti](https://azure.microsoft.com/support/legal/sla/) toobe platný.  

#### <a name="example-for-private-peering"></a>Příklad soukromého partnerského vztahu

Pokud si zvolíte a.b.c.d/29 tooset toouse až hello partnerský vztah, rozdělí se na dvě podsítě/30. V příkladu hello níže se podíváme na tom, jak hello se podsíť a.b.c.d/29 použije. 

a.b.c.d/29 bude tooa.b.c.d/30 rozdělení a a.b.c.d+4/30 a předá tooMicrosoft prostřednictvím hello zřizování rozhraní API. Použijete a.b.c.d+1 jako hello VRF IP pro hello primární PE a Microsoft spotřebuje a.b.c.d+2 jako IP adresu VRF pro hello hello primární MSEE. Použijete a.b.c.d+5 jako hello VRF IP pro hello sekundární PE a Microsoft použije a.b.c.d+6 jako IP adresu VRF pro hello hello sekundární MSEE.

Představte si případ, kdy můžete vybrat 192.168.100.128/29 tooset až soukromého partnerského vztahu. 192.168.100.128/29 obsahuje adresy od 192.168.100.128 too192.168.100.135, mezi které:

* 192.168.100.128/30 se přiřadí toolink1, kde poskytovatel použije 192.168.100.129 a Microsoft použije 192.168.100.130.
* 192.168.100.132/30 se přiřadí toolink2, kde poskytovatel použije 192.168.100.133 a Microsoft použije 192.168.100.134.

### <a name="ip-addresses-used-for-azure-public-and-microsoft-peering"></a>IP adresy sloužící pro veřejný partnerský vztah Azure a partnerský vztah Microsoftu

Pro nastavení relací protokolu BGP hello musíte použít veřejné IP adresy, které vlastníte. Microsoft musí být schopný tooverify hello vlastnictví hello IP adresy pomocí registrech Rir a IRR. 

* Musíte použít jedinečnou/29 podsíť nebo tooset dvě podsítě/30 až hello BGP partnerský vztah pro každý partnerský vztah na jeden okruh ExpressRoute (Pokud máte více než jeden). 
* Pokud se používá podsíť /29, rozdělí se na dvě podsítě /30. 
  * nejprve Hello/30 podsítě se použije pro primární propojení hello a hello druhý /30 podsítě se použije pro sekundární propojení hello.
  * Pro každé podsítě hello /30 musíte použít hello první IP adresu podsítě hello /30 na směrovači. Microsoft použije hello druhou IP adresu podsítě tooset hello /30 se relace protokolu BGP.
  * Musíte nastavit obě relace protokolu BGP pro naše [smlouva SLA o dostupnosti](https://azure.microsoft.com/support/legal/sla/) toobe platný.

## <a name="public-ip-address-requirement"></a>Požadavek veřejné IP adresy

### <a name="private-peering"></a>Soukromý partnerský vztah

Můžete toouse veřejných nebo privátních IPv4 adresy pro soukromý partnerský vztah. Poskytujeme ucelenou izolaci provozu, takže v případě soukromého partnerského vztahu není možné překrývání adres s jinými zákazníky. Tyto adresy nejsou inzerovaný tooInternet. 

### <a name="public-peering"></a>Veřejný partnerský vztah

Hello cesty veřejného partnerského vztahu Azure umožňuje vám tooconnect tooall služby hostované v Azure přes jejich veřejné IP adresy. Mezi ně patří služby uvedené v hello [Expressroute – nejčastější dotazy](expressroute-faqs.md) a veškeré služby hostované nezávislými dodavateli softwaru v Microsoft Azure. Připojení tooMicrosoft Azure služby na veřejného partnerského vztahu je vždycky iniciováno z vaší sítě do sítě Microsoft hello. Musíte použít veřejné IP adresy pro síť určených tooMicrosoft provoz hello.

### <a name="microsoft-peering"></a>Partnerský vztah Microsoftu

Cesta partnerského vztahu Microsoftu Hello vám umožní připojit se tooMicrosoft cloudové služby, které nejsou podporované prostřednictvím cesty veřejného partnerského vztahu Azure hello. Hello služby patří služby Office 365, jako je Exchange Online, SharePoint Online, Skype pro firmy a Dynamics 365. Společnost Microsoft podporuje obousměrné připojení na partnerský vztah Microsoftu hello. Přenosy určené tooMicrosoft cloudové služby musíte použít platné IPv4 adresy přeložené před vstupem do sítě Microsoft hello.

Ujistěte se, že vaše IP adresa a číslo AS jsou registrované tooyou v jednom z dál uvedených registrů hello.

* [ARIN](https://www.arin.net/)
* [APNIC](https://www.apnic.net/)
* [AFRINIC](https://www.afrinic.net/)
* [LACNIC](http://www.lacnic.net/)
* [RIPENCC](https://www.ripe.net/)
* [RADB](http://www.radb.net/)
* [ALTDB](http://altdb.net/)

> [!IMPORTANT]
> Veřejné IP adresy inzerované tooMicrosoft přes ExpressRoute nesmí být inzerovaný toohello Internetu. Tato akce může přerušit připojení tooother Microsoft služby. Nicméně veřejné IP adresy používané servery ve vaší síti, které komunikují s koncovými body O365 v rámci Microsoftu, lze inzerovat prostřednictvím ExpressRoute. 
> 
> 

## <a name="dynamic-route-exchange"></a>Dynamická výměna tras

Výměna směrování bude přes protokol EBGP. Relace EBGP se vytvoří mezi hello Msee a vašimi směrovači. Ověřování relací BGP není povinné. V případě potřeby lze nakonfigurovat hodnotu hash MD5. V tématu hello [konfigurace směrování](expressroute-howto-routing-classic.md) a [pracovní postupy zřizování okruhů a stavy okruhu](expressroute-workflows.md) informace o konfiguraci relací BGP.

## <a name="autonomous-system-numbers"></a>Čísla autonomního systému

Microsoft pro veřejný partnerský vztah Azure, soukromý partnerský vztah Azure a partnerský vztah Microsoftu použije AS 12076. Jsme vyhradili čísla ASN od 65515 too65520 pro interní použití. Jsou podporována 16bitová a 32bitová čísla AS.

Nejsou žádné požadavky týkající se symetrie přenosu dat. Hello cesty vpřed a zpět můžou procházet různými dvojicemi směrovačů. Můžou být inzerovány identické trasy z obou stran přes víc dvojic okruhů, které vám patří. Metriky tras nejsou požadované toobe identické.

## <a name="route-aggregation-and-prefix-limits"></a>Agregace tras a omezení předpon

Podporujeme až too4000 předpony inzerované prostřednictvím soukromého partnerského vztahu Azure hello toous. To může být zvýšena až too10 000 předpon, pokud je povolen doplněk ExpressRoute premium hello. Můžeme přijmout too200 předpon na každou relaci BGP pro veřejný Azure a partnerský vztah Microsoftu. 

relace protokolu BGP Hello se zahodí, pokud hello počet předpon překročí hello limit. Budeme přijímat výchozí trasy na hello propojeních soukromého partnerského vztahu pouze. Poskytovatel musí odfiltrovat výchozí trasy a privátní IP adresy (RFC 1918) z hello veřejný Azure a cesty partnerského vztahu Microsoftu. 

## <a name="transit-routing-and-cross-region-routing"></a>Tranzitní směrování a směrování mezi oblastmi

Službu ExpressRoute nejde nakonfigurovat jako tranzitní směrovače. Toorely bude mít na svého poskytovatele připojení ohledně služeb tranzitního směrování.

## <a name="advertising-default-routes"></a>Inzerování výchozích tras

Výchozí trasy jsou povolené jenom na relacích soukromého partnerského vztahu Azure. V takovém případě bude směrovat veškerý provoz ze sítě tooyour hello přidružené virtuální sítě. Inzerování výchozích tras do soukromého partnerského vztahu povede hello internetové trasy z Azure budou blokovány. Musíte spoléhat na vaše podnikové hraniční tooroute provoz z a toohello Internetu pro služby hostované v Azure. 

 tooenable připojení tooother Azure services a služby infrastruktury, je nutné nejprve jeden z následujících položek hello je na místě:

* Veřejný partnerský vztah Azure je povoleno tooroute provoz toopublic koncové body
* Uživatelem definované směrování tooallow připojení k Internetu se používá pro každou podsíť vyžadují připojení k Internetu.

> [!NOTE]
> Inzerování výchozích tras poruší aktivaci licencí pro Windows a jiné virtuální počítače. Postupujte podle pokynů [sem](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx) toowork řešení.
> 
> 

## <a name="support-for-bgp-communities-preview"></a>Podpora komunit protokolu BGP (Preview)

Tato část obsahuje přehled použití komunit protokolu BGP se službou ExpressRoute. Microsoft bude Inzerovat trasy v hello veřejné a cesty partnerského vztahu Microsoftu s trasami, které jsou označené odpovídajícími hodnotami komunity. Hello důvody tohoto postupu a hello podrobnosti o komunity, které hodnoty jsou popsány níže. Microsoft, ale nebude ctít žádné komunity hodnoty s příznakem tooroutes inzerované tooMicrosoft.

Pokud se připojujete tooMicrosoft prostřednictvím ExpressRoute v libovolném jeden umístění partnerského vztahu v rámci geopolitické oblasti, budete mít cloudových služeb přístup tooall Microsoftu přes všechny oblasti v rámci geopolitické hranice hello. 

Například pokud jste tooMicrosoft v Amsterdamu připojené prostřednictvím ExpressRoute, bude mít přístup tooall cloudovým službám Microsoftu hostovaným v oblastech severní Evropa a západní Evropa. 

Odkazovat toohello [ExpressRoute partnery a umístění partnerského vztahu](expressroute-locations.md) stránky pro podrobný seznam geopolitických oblastí, přidružených oblastí Azure a odpovídajících umístění partnerského vztahu ExpressRoute.

Můžete zakoupit víc než jeden okruh ExpressRoute na geopolitickou oblast. Použití víc připojení nabízí významné výhody vysoké dostupnosti kvůli toogeo redundance. V případech, kdy máte víc okruhů ExpressRoute obdržíte stejnou sadu předpon inzerovaných hello od společnosti Microsoft na hello veřejného partnerského vztahu a partnerského vztahu Microsoftu. To znamená, že bude mít z vaší sítě do Microsoftu víc cest. To může potenciálně způsobit optimální směrování rozhodnutí toobe provedené v rámci vaší sítě. V důsledku toho můžete setkat s neoptimálním průběhem připojení prostředí toodifferent služby. 

Microsoft označí předpony inzerované prostřednictvím veřejného partnerského vztahu a partnerského vztahu příslušné hodnoty komunity protokolu BGP označující hello oblast hello předpony Microsoftu jsou hostované v. Můžete spoléhat na hello komunity hodnoty toomake odpovídající směrování rozhodnutí toooffer [optimální směrování toocustomers](expressroute-optimize-routing.md).

| **Geopolitická oblast** | **Oblast Microsoft Azure** | **Hodnota komunity protokolu BGP** |
| --- | --- | --- |
| **Severní Amerika** | | |
| Východ USA |12076:51004 | |
| Východní USA 2 |12076:51005 | |
| Západní USA |12076:51006 | |
| Západní USA 2 |12076:51026 | |
| Západní střed USA |12076:51027 | |
| Střed USA – sever |12076:51007 | |
| Střed USA – jih |12076:51008 | |
| Střed USA |12076:51009 | |
| Střední Kanada |12076:51020 | |
| Východní Kanada |12076:51021 | |
| **Jižní Amerika** | | |
| Brazílie – jih |12076:51014 | |
| **Evropa** | | |
| Severní Evropa |12076:51003 | |
| Západní Evropa |12076:51002 | |
| **Asie a Tichomoří** | | |
| Východní Asie |12076:51010 | |
| Jihovýchodní Asie |12076:51011 | |
| **Japonsko** | | |
| Japonsko – východ |12076:51012 | |
| Japonsko – západ |12076:51013 | |
| **Austrálie** | | |
| Austrálie – východ |12076:51015 | |
| Austrálie – jihovýchod |12076:51016 | |
| **Indie** | | |
| Indie – jih |12076:51019 | |
| Indie – západ |12076:51018 | |
| Indie – střed |12076:51017 | |

Všechny trasy inzerované Microsoftem budou označené hello odpovídající hodnotou komunity. 

> [!IMPORTANT]
> Globální předpony budou označené odpovídající hodnotou komunity a budou se inzerovat, jenom když bude povolený doplněk ExpressRoute Premium.
> 
> 

Kromě výše uvedených toohello, bude Microsoft také označovat předpony podle služby hello, ke které patří. To platí, pouze toohello partnerský vztah Microsoftu. Následující tabulka Hello poskytuje mapování služby tooBGP komunity hodnoty.

| **Služba** | **Hodnota komunity protokolu BGP** |
| --- | --- |
| **Výměna** |12076:5010 |
| **SharePoint** |12076:5020 |
| **Skype pro firmy** |12076:5030 |
| **Dynamics 365** |12076:5040 |
| **Jiné služby Office 365** |12076:5100 |

> [!NOTE]
> Microsoft nectí žádné hodnoty komunity protokolu BGP, která jste nastavili na hello trasy inzerované tooMicrosoft.
> 
> 

## <a name="next-steps"></a>Další kroky

* Nakonfigurujte připojení ExpressRoute.
  
  * [Vytvoření okruhu ExpressRoute pro model nasazení classic hello](expressroute-howto-circuit-classic.md) nebo [vytvoření a úprava okruhu ExpressRoute pomocí Azure Resource Manager](expressroute-howto-circuit-arm.md)
  * [Konfigurace směrování pro model nasazení classic hello](expressroute-howto-routing-classic.md) nebo [konfigurace směrování pro model nasazení Resource Manager hello](expressroute-howto-routing-arm.md)
  * [Odkaz klasické virtuální sítě tooan okruh ExpressRoute](expressroute-howto-linkvnet-classic.md) nebo [propojení virtuální sítě Resource Manageru tooan okruh ExpressRoute](expressroute-howto-linkvnet-arm.md)

