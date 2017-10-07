---
title: "aaaNetwork koncepty zabezpečení & požadavků v Azure | Microsoft Docs"
description: " Tento článek usnadňuje jste toounderstand co Microsoft Azure má toooffer v oblasti hello zabezpečení sítě. Poskytujeme základní vysvětlení požadavky a základní koncepty zabezpečení sítě a informace o jaké Azure má toooffer v každé z těchto oblastí. "
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TomSh
ms.assetid: bedf411a-0781-47b9-9742-d524cf3dbfc1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: terrylan
ms.openlocfilehash: 87d336064b880ddcf90ae4fcb79b7823367682b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-network-security-overview"></a>Přehled zabezpečení sítě Azure
Microsoft Azure obsahuje robustní sítě toosupport infrastruktury, aplikace a požadavky na připojení služby. Připojení k síti je možné mezi prostředky, které jsou umístěné v Azure, mezi místními a Azure hostované prostředky a tooand z hello Internet a Azure.

Hello cílem tohoto článku je toomake usnadňuje toounderstand jste ji co Microsoft Azure má toooffer v oblasti hello zabezpečení sítě. Zde jsou základní vysvětlení požadavky a základní koncepty zabezpečení sítě. Poskytujeme také informace o jaké Azure má v každé z těchto oblastí toooffer a také odkazy toohelp získat lepší představu o zajímavé oblasti.

Tento přehled zabezpečení sítě Azure článek se zaměřuje na hello následující oblasti:

* Síť Azure
* Řízení přístupu k síti
* Zabezpečení vzdáleného přístupu a mezi různými místy připojení
* Dostupnost
* Překlad adres
* Architektura hraniční sítě
* Monitorování a zjišťování hrozeb


## <a name="azure-networking"></a>Sítě Azure
Třeba virtuální počítače připojení k síti. toosupport tento požadavek Azure vyžaduje toobe virtuální počítače připojené tooan Azure Virtual Network. Virtuální síť Azure je logická konstrukce nástavbou hello Azure síťových prostředcích infrastruktury. Každé logické síti Azure Virtual Network je izolovaná od všech ostatních Azure virtuálních sítí. To pomáhá zajistit, že síťový provoz v nasazeních není přístupný tooother Microsoft Azure zákazníků.

Další informace:

* [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md)


## <a name="network-access-control"></a>Řízení přístupu k síti
Řízení přístupu k síti je hello v rámci omezení tooand připojení z konkrétní zařízení nebo podsítí ve virtuální síti Azure. cílem Hello řízení přístupu k síti je toolimit přístup tooyour virtuální počítače a služby tooapproved uživatelů a zařízení. Řízení přístupu jsou založené na povolit nebo odepřít rozhodnutí, která pro připojení tooand od virtuálního počítače nebo služby.

Azure podporuje několik typů řízení přístupu k síti, jako:

* Vrstva síťového ovládacího prvku
* Směrování řízení a vynuceného tunelování
* Virtuální síť zabezpečovací zařízení

### <a name="network-layer-control"></a>Vrstva síťového ovládacího prvku
Žádné zabezpečení nasazení vyžaduje určitou míru řízení přístupu k síti. cílem Hello řízení přístupu k síti je toorestrict virtuální počítač komunikace toohello nezbytných systémů a že jsou zablokované další pokusy o komunikaci.

Pokud potřebujete řízení úrovně přístupu k základní síti (na základě IP adresy a hello TCP nebo UDP protokoly), můžete použít skupiny zabezpečení sítě. Skupina zabezpečení sítě (NSG) je základní stavových paketů filtrování brány firewall a umožňuje toocontrol přístup na základě [5 řazené kolekce členů](https://www.techopedia.com/definition/28190/5-tuple). Skupiny Nsg neposkytují kontroly vrstvy aplikací nebo ověřený řízení přístupu.

Další informace:

* [Skupiny zabezpečení sítě](../virtual-network/virtual-networks-nsg.md)

### <a name="route-control-and-forced-tunneling"></a>Řízení směrování a vynucené tunelování
Hello možnost toocontrol chování směrování na virtuálních sítí Azure je kritických síťových funkce řízení zabezpečení a přístup. Pokud směrování není správně nakonfigurovaná, aplikace a služby hostované na virtuálním počítači může připojit toounauthorized zařízení, včetně systémů vlastníte a která je provozována společností potenciálním útočníkům.

Azure sítě podporuje hello možnost toocustomize hello směrování chování pro síťový provoz na virtuálních sítí Azure. To vám umožní tooalter hello výchozí směrovacích tabulek ve virtuální síti Azure. Řízení chování směrování díky tomu můžete zajistit, aby veškerý provoz z určitého zařízení nebo skupinu zařízení zadá nebo opustí vaši virtuální síť pomocí konkrétního umístění.

Například můžete mít zabezpečení zařízení virtuální sítě ve vaší virtuální síti Azure. Chcete toomake jistotu, že všechny přenosy tooand z vaší virtuální sítě Azure prochází této virtuální zabezpečení zařízení. To provedete tak, že nakonfigurujete [trasy definované uživatelem](../virtual-network/virtual-networks-udr-overview.md) v Azure.

[Vynuceného tunelování](https://www.petri.com/azure-forced-tunneling) je mechanismus, který můžete použít tooensure, které vaše služby nejsou povoleny tooinitiate toodevices připojení na hello Internetu. Tento název se liší od přijímat příchozí připojení a pak reagovat toothem. Webových serverů front-end potřebovat toorespond toorequests z hostitelů v Internetu, a proto Source internetové komunikace povolena příchozí toothese webové servery a webové servery hello jsou povoleny toorespond.

Co nechcete, aby tooallow je front-end webovém serveru tooinitiate výstupního požadavku. Tyto žádosti může představovat riziko zabezpečení, protože tato připojení může být použité toodownload malwaru. I když chcete tyto front-end servery tooinitiate odchozí požadavky toohello Internet, je vhodné tooforce je toogo prostřednictvím místní webové proxy servery tak, aby můžete využít výhod adresu URL pro filtrování a protokolování.

Místo toho je vhodnější, že toouse vynutit tunelové tooprevent to. Když povolíte vynucené tunelování, jsou všechny připojení toohello Internet vynutit prostřednictvím vaší místní brány. Můžete konfigurovat vynucené tunelování využitím trasy definované uživatelem.

Další informace:

* [Co jsou trasy definované uživatelem a předávání IP](../virtual-network/virtual-networks-udr-overview.md)

### <a name="virtual-network-security-appliances"></a>Virtuální síť zabezpečovací zařízení
Zatímco skupin zabezpečení sítě, trasy definované uživatelem a vynucené tunelování poskytují úroveň zabezpečení v hello sítě a transportní vrstvy hello [OSI model](https://en.wikipedia.org/wiki/OSI_model), může nastat situace, kdy chcete tooenable zabezpečení na vyšší úroveň než hello síť.

Například požadavky na zabezpečení mohou zahrnovat:

* Ověřování a autorizace před povolením přístupu tooyour aplikace
* Zjišťování neoprávněných vniknutí a narušení odpovědi
* Kontroly vrstvy aplikace pro protokoly vysoké úrovně
* Filtrování adres URL
* Sítě úrovně antivirového a antimalwarového
* Ochrana proti robota
* Řízení přístupu aplikace
* Další ochrana proti útoku DDoS (nad hello DDoS ochrany hello prostředků infrastruktury Azure sám sebe)

Tyto funkce Rozšířené sítě zabezpečení lze využívat pomocí služby Azure partnerských řešení. Můžete najít hello nejaktuálnější řešení zabezpečení sítě partnera Azure návštěvou hello [Azure Marketplace](https://azure.microsoft.com/marketplace/) a při vyhledání "zabezpečení" a "zabezpečení sítě."

## <a name="secure-remote-access-and-cross-premises-connectivity"></a>Zabezpečený vzdálený přístup a připojení mezi více místy
Instalace, konfigurace a Správa prostředků Azure potřebuje toobe provést vzdáleně. Kromě toho může být vhodné toodeploy [hybridního IT](http://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) řešení, které mají součásti na místě a v hello veřejného cloudu Azure. Tyto scénáře vyžadují zabezpečený vzdálený přístup.

Síť Azure podporuje následující scénáře zabezpečený vzdálený přístup hello:

* Připojení jednotlivých pracovních stanicích tooan Azure Virtual Network
* Připojení vaší místní síti tooan Azure Virtual Network prostřednictvím sítě VPN
* Spojte se s vyhrazenou propojení WAN tooan vaší místní síti Azure Virtual Network
* Připojit virtuální sítě Azure tooeach jiné

### <a name="connect-individual-workstations-tooan-azure-virtual-network"></a>Připojení jednotlivých pracovních stanicích tooan Azure Virtual Network
Může nastat situace, kdy chcete tooenable jednotlivých vývojáře nebo operations pracovníky toomanage virtuální počítače a služby v Azure. Například potřebovat přístup k tooa virtuálního počítače na virtuální síť Azure a vaše zásady zabezpečení nepovoluje protokolu RDP nebo SSH vzdáleného přístupu tooindividual virtuálních počítačů. V takovém případě můžete použít připojení point-to-site VPN.

Hello point-to-site VPN připojení používá hello [SSTP VPN](https://technet.microsoft.com/library/cc731352.aspx) protokol tooenable tooset se privátní a bezpečné připojení mezi hello uživatele a hello Azure Virtual Network. Po vytvoření připojení VPN hello hello uživatel nebude moct tooRDP nebo SSH přes hello VPN odkaz na jakýkoli virtuální počítač na hello Azure virtuální sítě (za předpokladu, že hello uživatel může ověřit je a oprávnění).

Další informace:

* [Konfigurace připojení Point-to-Site tooa, virtuální sítě pomocí prostředí PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="connect-your-on-premises-network-tooan-azure-virtual-network-with-a-vpn"></a>Připojit vaše místní síť tooan Azure Virtual Network prostřednictvím sítě VPN
Můžete chtít tooconnect celý podnikové sítě nebo její části, tooan Azure Virtual Network. To je běžné v hybridní IT scénáře kde společnosti [rozšířit jejich místní datacentra do Azure](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84). V mnoha případech bude společností hostitelem součástí služby v Azure a částí místně, například při řešení zahrnuje front-end webové servery v Azure a back-end databáze na místě. Tyto typy připojení "mezi různými místy" rovněž usnadňují správu Azure, které jsou umístěné prostředky víc zabezpečit a povolit scénáře, jako je například rozšíření řadiče domény služby Active Directory do Azure.

Jedním ze způsobů tooaccomplish jde toouse [site-to-site VPN](https://www.techopedia.com/definition/30747/site-to-site-vpn). Hello rozdíl mezi VPN site-to-site a point-to-site VPN je, že síť point-to-site VPN připojí jedno zařízení tooan Azure Virtual Network, při site-to-site VPN se připojuje celé síti (například síti na pracovišti) tooan Azure Virtual Network . Site-to-site VPN tooan Azure Virtual Network pomocí hello vysoce zabezpečených tunel režimu VPN protokolu IPsec.

Další informace:

* [Vytvoření virtuální sítě Resource Manager s připojením VPN typu site-to-site pomocí hello portálu Azure](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [Plánování a návrh pro bránu VPN](../vpn-gateway/vpn-gateway-plan-design.md)

### <a name="connect-your-on-premises-network-tooan-azure-virtual-network-with-a-dedicated-wan-link"></a>Spojte se s propojení WAN vyhrazené tooan vaší místní síti Azure Virtual Network
Připojení k síti VPN Point-to-site a site-to-site jsou platné pro povolení připojení mezi různými místy. Ale některé organizace zvažte je toohave hello následující nevýhody:

* Připojení k síti VPN přesouvání dat v hello Internetu – to zpřístupňuje těchto připojení toopotential problémy se zabezpečením zabývajících se přesouvání dat přes veřejnou síť. Kromě toho nemůže být zaručit spolehlivost a dostupnost pro připojení k Internetu.
* TooAzure připojení VPN virtuální sítě může být považovaný za šířka pásma omezená pro některé aplikace a účely, jakmile jsou maximální limit přibližně 200 MB/s.

Organizace, které potřebují hello nejvyšší úrovně zabezpečení a dostupnosti pro jejich připojení mezi různými místy obvykle používat vyhrazené linky WAN tooconnect tooremote weby. Azure poskytuje že Hello možnost toouse vyhrazené propojení WAN, které můžete použít tooconnect tooan vaší místní síti Azure Virtual Network. Tato možnost je povolena prostřednictvím Azure ExpressRoute.

Další informace:

* [Technický přehled ExpressRoute](../expressroute/expressroute-introduction.md)

### <a name="connect-azure-virtual-networks-tooeach-other"></a>Připojit virtuální sítě Azure tooEach jiné
Je možné, můžete toouse mnoho virtuálních sítí Azure pro vaše nasazení. Existuje mnoho důvodů, proč to můžete udělat. Jedním z důvodů hello může být správa toosimplify; z bezpečnostních důvodů může být jiné. Bez ohledu na hello motivace nebo důvody uvedení prostředky v různých virtuálních sítích Azure může být časy, kdy chcete prostředky na každém tooconnect sítě hello sebou.

Jednou z možností pro služby v jedné síti Azure Virtual Network tooconnect tooservices na jinou virtuální sítí Azure by tím"zpět" prostřednictvím hello Internetu. připojení Hello by spustit na jednu virtuální síť Azure, přejděte přes hello Internetu a pak se vraťte toohello cílové síti Azure Virtual Network. Tato možnost zpřístupní hello připojení toohello zabezpečení problémy vyplývajících tooany internetové komunikace.

Lepší volbou může být toocreate Azure virtuální sítě Azure virtuální sítě site-to-site VPN. Tato virtuální síť Azure virtuální sítě Azure VPN site-to-site používá hello stejné [režimu tunelového připojení IPsec](https://technet.microsoft.com/library/cc786385.aspx) protokol jako hello mezi různými místy site-to-site VPN připojení uvedených výše.

Výhodou Hello pomocí služby Azure virtuální sítě Azure virtuální sítě site-to-site VPN je, že hello připojení VPN je vytvořeno přes hello Azure síťových prostředcích infrastruktury místo připojující se přes hello Internet. To poskytuje že další vrstvu zabezpečení můžete porovnat toosite-to-site VPN, které se připojují přes hello Internet.

Další informace:

* [Nakonfigurujte připojení VNet-to-VNet s použitím Azure Resource Manageru a prostředí PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="availability"></a>Dostupnost
Dostupnost je klíčovou součástí žádné program zabezpečení. Pokud uživatelé a systémy nemají přístup k jaké budou potřebovat tooaccess přes hello síti hello služby lze považovat za narušení. Azure má tuto podporu hello následující mechanismy vysoké dostupnosti síťových technologií:

* Vyrovnávání zatížení založené na protokolu HTTP
* Úrovně služby Vyrovnávání zatížení sítě
* Globální Vyrovnávání zatížení

Vyrovnávání zatížení je mechanismus určený tooequally distribuci připojení mezi různými zařízeními. Hello cíle Vyrovnávání zatížení jsou:

* Zvýšit dostupnost – když zavedete vyrovnávání připojení mezi různými zařízeními, jeden nebo více zařízení hello můžete k dispozici a hello služby spuštěné na hello zbývající online zařízení můžete pokračovat tooserve hello obsah ze služby hello
* Zvýšení výkonu – při načítání vyrovnávání připojení mezi různými zařízeními, jedno zařízení nemá tootake hello procesoru přístupů. Místo toho hello zpracování a paměti požadavky pro poskytování obsahu hello je rozdělena mezi různými zařízeními.

### <a name="http-based-load-balancing"></a>Vyrovnávání zatížení založené na protokolu HTTP
Organizace, webových služeb používají často desire toohave k nástroji pro vyrovnávání zatížení založené na protokolu HTTP před tyto webové služby toohelp zajistěte odpovídající úrovně výkonu a vysokou dostupnost. Na rozdíl od nástroje pro vyrovnávání zatížení tootraditional založené na síti, hello rozhodnutí Vyrovnávání zatížení provedené nástroje pro vyrovnávání zatížení založené na protokolu HTTP jsou založené na vlastnosti protokolu hello HTTP, nikoli na protokoly hello sítě a transportní vrstvy.

tooprovide je založený na protokolu HTTP Vyrovnávání zatížení pro vaše webové služby Azure poskytuje hello Azure Application Gateway. podporuje Azure Application Gateway Hello:

* Založené na protokolu HTTP Vyrovnávání zatížení – rozhodnutí o vyrovnávání zatížení jsou na základě protokolu charakteristik speciální toohello HTTP
* Spřažení na základě souboru cookie relace – tato funkce je zajištěno, že připojení navázat tooone hello serverů za tento nástroj pro vyrovnávání zatížení zůstane beze změn mezi hello klientem a serverem. Díky tomu se budou stabilitu transakce.
* Přesměrování zpracování SSL – při připojení klienta se naváže hello Vyrovnávání zatížení, aby relace mezi hello klienta a nástroj pro vyrovnávání zatížení hello se šifruje pomocí hello HTTPS (SSL nebo) protokolu. Ale v pořadí tooincrease výkon, máte hello možnost toohave hello připojení mezi hello nástroj pro vyrovnávání zatížení a webový server hello za protokol HTTP (bez šifrování) hello použití vyrovnávání hello zátěže. Toto je odkazované tooas "přesměrování zpracování SSL", protože hello webové servery za nástroj pro vyrovnávání zatížení hello nemáte prostředí nároky na procesor hello, které jsou spojené s šifrování a proto by měl být schopný tooservice požadavky rychleji.
* Adresa URL obsahu směrování podle – tato funkce umožňuje hello rozhodnutí o toomake nástroje pro vyrovnávání zatížení na kde tooforward připojení podle hello cílová adresa URL. To poskytuje flexibilnější než u řešení, které načíst vyrovnávání rozhodnutí, která na základě IP adres.

Další informace:

* [Přehled brány aplikace](../application-gateway/application-gateway-introduction.md)

### <a name="network-level-load-balancing"></a>Úrovně služby Vyrovnávání zatížení sítě
Na rozdíl od na základě tooHTTP Vyrovnávání zatížení, úrovně služby Vyrovnávání zatížení sítě umožňuje načíst vyrovnávání rozhodnutí založená na IP adresu a port (TCP nebo UDP) čísla.
Je možné získat výhody hello úrovně služby Vyrovnávání zatížení sítě v Azure pomocí hello Vyrovnávání zatížení Azure. Některé klíčové vlastnosti hello nástroj pro vyrovnávání zatížení Azure patří:

* Vyrovnávání zatížení úrovně sítě podle IP adresy a portu čísla
* Podpora pro protokol vrstvy jakékoli aplikace
* Vyrovnává zatížení tooAzure virtuálních počítačů a cloudových služeb instance rolí
* Lze použít pro internetové (Vyrovnávání zatížení externí) i bez Internetu facing (interní Vyrovnávání zatížení) aplikace a virtuální počítače
* Koncový bod monitorování, která je použité toodetermine, pokud některé z hello služeb za nástroj pro vyrovnávání zatížení hello mít k dispozici

Další informace:

* [Nástroj pro vyrovnávání zatížení z internetu mezi několika virtuálními počítači nebo službami](../load-balancer/load-balancer-internet-overview.md)
* [Přehled nástroje pro vyrovnávání zatížení interní](../load-balancer/load-balancer-internal-overview.md)

### <a name="global-load-balancing"></a>Globální Vyrovnávání zatížení
Některé organizace bude chtít hello nejvyšší úroveň dostupnosti možné. Jedním ze způsobů tooreach tento cíl je toohost aplikace v globálně distribuované datových centrech. Když aplikace hostovaný v datových centrech umístěné v rámci hello, world, je možné pro toobecome celý geopolitické oblasti není k dispozici a stále mít aplikace hello a spuštěná.

Kromě toho toohello dostupnosti výhody, které dostanete tak, že hostování aplikací v datových centrech globálně distribuované, také můžete získat výhody výkonu. Tyto výhody výkonu lze získat pomocí mechanismus, který přesměruje požadavky pro hello služby toohello datacenter, které je nejblíže toohello zařízení, která odeslala žádost hello.

Vyrovnávání zatížení globální vám může poskytnout obě tyto výhody. V Azure můžete získat výhody hello globální Vyrovnávání zatížení pomocí Azure Traffic Manager.

Další informace:

* [Co je Traffic Manager?](../traffic-manager/traffic-manager-overview.md)


## <a name="name-resolution"></a>Překlad názvů
Překlad je důležité funkce pro všechny služby, které je hostovat v Azure. Z hlediska zabezpečení může způsobit ohrožení hello název řešení funkce z vaší lokality tooan útočník lokality, tooan útočník přesměrování požadavků. Zabezpečené překlad je požadavek pro všechny hostované cloudové služby.

Existují dva typy překladu adres, které že budete potřebovat tooaddress:

* Interní překlad adres – interní překlad adres se používá služby v Azure Virtual Network, místní sítě nebo obojí. Názvy používaných pro interní překlad adres nejsou dostupné přes hello Internetu. Pro optimální zabezpečení je důležité, vaše interní název řešení schématu není přístupný tooexternal uživatele.
* Externí název řešení – externí překlad názvů používá uživatele a zařízení mimo místní a virtuální sítí Azure. Jedná se o hello názvy, které jsou viditelné toohello Internet a jsou použité toodirect připojení tooyour cloudové služby.

Pro interní překlad adres máte dvě možnosti:

* Serveru DNS virtuální sítě Azure – při vytváření nové virtuální sítě Azure, server DNS se vytvoří pro vás. Tento server DNS může překládat názvy hello hello počítačů, které jsou umístěné ve virtuální síti Azure. Tento server DNS není Konfigurovatelný a spravuje správce prostředků infrastruktury Azure hello, a tím řešení zabezpečeného název řešení.
* Přineste vlastní server DNS – máte možnost hello uvedení server DNS podle vlastní volby ve vaší virtuální síti Azure. Tento server DNS může být, že integraci služby Active Directory DNS server, nebo vyhrazený řešení, server DNS poskytovaný Azure partnera, který můžete získat z hello Azure Marketplace.

Další informace:

* [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md)
* [Správa serverů DNS používaných virtuální síť (VNet)](../virtual-network/virtual-network-manage-network.md#dns-servers)

Pro externí překlad DNS máte dvě možnosti:

* Hostování vlastní externí DNS místního serveru
* Hostování externím serverem DNS s poskytovatelem služeb

Mnoho velkých organizacích bude hostovat vlastní DNS servery místně. Protože mají hello sítě znalosti a globální přítomnosti toodo proto to dělají.

Ve většině případů je lepší toohost vaše překlad názvu DNS služeb s poskytovatelem služeb. Poskytovatelé služeb mají znalosti hello sítě a globální přítomnosti tooensure velmi vysokou dostupnost pro vaše řešení služby název. Dostupnost je nezbytné DNS služeb, protože pokud vaše překlad služby nezdaří, bude nikdo být schopný tooreach vaší internetové služby.

Azure poskytuje s vysokou dostupností a původce externí DNS řešení v podobě hello Azure DNS. Toto řešení externí název řešení využívá výhod infrastruktury Azure DNS po celém světě hello. Umožňuje toohost doménu v Azure pomocí hello stejné přihlašovací údaje, rozhraní API, nástroje a fakturace jako jinými službami Azure. V rámci Azure je rovněž dědí hello silné zabezpečení ovládacích prvků, součástí platformy hello.

Další informace:

* [Přehled Azure DNS](../dns/dns-overview.md)

## <a name="dmz-architecture"></a>Architektura hraniční sítě
Mnoho organizací enterprise používá zóny DMZ toosegment jejich toocreate sítě zónu vyrovnávací paměti mezi hello Internet a službám. Hello DMZ část hello sítě se považuje za zóny s nízkou úrovní zabezpečení a žádné prostředky vysoké hodnoty jsou umístěny v tomto segmentu sítě. Obvykle se zobrazí sítě zabezpečovací zařízení, které mají síťové rozhraní v segmentu hello DMZ a jiné síti připojené tooa rozhraní sítě, která má virtuální počítače a služby, které přijímají příchozí připojení z Internetu hello.

Existuje několik odchylek DMZ návrhu a rozhodnutí toodeploy hello DMZ a jaký typ DMZ toouse Pokud se rozhodnete toouse, je na základě požadavků zabezpečení vaší sítě.

Další informace:

* [Cloudové služby společnosti Microsoft a zabezpečení sítě](../best-practices-network-security.md)


## <a name="monitoring-and-threat-detection"></a>Monitorování a zjišťování hrozeb

Azure poskytuje možnosti toohelp v této oblasti klíče s časná zjišťování, monitorování a hello možnost toocollect a zkontrolujte síťový provoz.

### <a name="azure-network-watcher"></a>Sledovací proces sítě Azure
Azure sledovací proces sítě obsahuje velké množství funkcí, které pomáhají při řešení problémů a také poskytují sadu nástrojů tooassist zcela nový hello identifikace problémy se zabezpečením.

[Zobrazení skupiny zabezpečení ](/network-watcher/network-watcher-security-group-view-overview.md) pomáhá s auditování a zabezpečení dodržování předpisů virtuálních počítačů a může být použité tooperform programový audity porovnávání hello směrné plány zásady definované ve vaší organizaci tooeffective pravidla pro jednotlivé virtuální počítače. Můžete identifikovat všechny odlišily konfigurace.

[Zachytáváním paketů](/network-watcher/network-watcher-packet-capture-overview.md) vám umožní toocapture síťový provoz tooand z hello virtuálního počítače. Kromě toho, tedy zvýšení tak, že umožní toocollect statistiku sítě a řešení potíží s hello aplikace mohou být neocenitelnou pomocí při šetření hello sítě vniknutí zachytáváním paketů problémy. Můžete také použít tuto funkci společně s Azure Functions toostart síťové přenosy v odpovědi toospecific Azure výstrahy.

Další informace o sledovací proces sítě Azure a jak toostart některých funkcí hello testování v vaší laboratoře prohlédněte hello [sledovací proces sítě Azure Přehled monitorování](/network-watcher/network-watcher-monitoring-overview.md)

>[!NOTE]
Azure sledovací proces sítě je stále ve verzi public preview, nemusí mít hello stejnou úroveň dostupnost a spolehlivost jako služby, které jsou obecně dostupnosti verze. Některé funkce nemusí být podporována, může mít omezené možnosti a nemusí být k dispozici ve všech Azure umístění. Hello nejaktuálnější oznámení o dostupnosti a stav této služby, zkontrolujte hello [Azure aktualizace stránky](https://azure.microsoft.com/updates/?product=network-watcher)

### <a name="azure-security-center"></a>Azure Security Center
Security Center pomáhá zabránit, zjistit a reagovat toothreats a poskytuje že vyšší přehled a kontrolu nad, hello zabezpečení vašich prostředků Azure. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných Azure, pomáhá zjišťovat hrozby, kterých byste jinak nevšimli a spolupracuje s velké sady řešení zabezpečení.

Azure Security Center pomáhá optimalizovat a monitorování zabezpečení sítě podle:

* Poskytuje doporučení zabezpečení sítě
* Monitorování stavu hello konfigurace zabezpečení sítě
* Výstrahy upozorňující na základě toonetwork hrozeb obou úrovni hello koncový bod a sítě

Další informace:

* [Úvod tooAzure Security Center](../security-center/security-center-intro.md)


### <a name="logging"></a>Protokolování
Protokolování na úrovni sítě je klíčovou funkcí pro každý scénář zabezpečení sítě. V Azure můžete protokolovat informace získané pro skupiny zabezpečení sítě tooget sítě úroveň protokolování informací. S protokolováním NSG můžete získat informace z:

* [Protokoly aktivity](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) – tyto protokoly jsou použité tooview všechny operace odeslání tooyour Azure odběry. Tyto protokoly jsou ve výchozím nastavení povolené a dá se použít hello portálu Azure. Byly dřív označovala jako "Protokoly auditu" nebo "Operační protokoly".
* Protokoly událostí – tyto protokoly poskytují informace o jaké pravidla NSG byly použity.
* Čítač protokoly – tyto protokoly umožní víte, kolikrát byl každého pravidla NSG použitá toodeny nebo povolení provozu.

Můžete také použít [Microsoft Power BI](https://powerbi.microsoft.com/what-is-power-bi/), vizualizaci dat výkonný nástroj tooview a tyto protokoly analyzovat.

Další informace:

* [Analýzy protokolů pro skupinu zabezpečení sítě (Nsg)](../virtual-network/virtual-network-nsg-manage-log.md)
