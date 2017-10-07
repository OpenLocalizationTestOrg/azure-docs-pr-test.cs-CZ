---
title: "osvědčené postupy zabezpečení sítě aaaAzure | Microsoft Docs"
description: "Další informace, že některé klíčové funkce hello k dispozici v Azure toohelp vytvořit zabezpečené sítě v prostředích"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: d169387a-1243-4867-a602-01d6f2d8a2a1
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: b851b2862428a8bd5e7525c85584fc1c14ffcabe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-cloud-services-and-network-security"></a>Zabezpečení služeb a sítě cloudu Microsoft
Cloudové služby společnosti Microsoft poskytování služeb flexibilně škálovatelné a infrastruktury, podnikové úrovni funkce a mnoho možností pro hybridní připojení. Zákazníci zvolit tooaccess tyto služby prostřednictvím hello Internetu nebo s Azure ExpressRoute, které poskytuje připojení k privátní síti. Hello platformy Microsoft Azure umožňuje zákazníkům tooseamlessly rozšířit svoji infrastrukturu do cloudu hello a vytvářet vícevrstvé architektury. Kromě toho můžete povolit třetím stranám rozšířené možnosti prostřednictvím nabídky zabezpečení služeb a virtuálních zařízení. Tento dokument poskytuje přehled o zabezpečení a architektury problémy, které zákazníci by měli zvážit při používání cloudové služby společnosti Microsoft získat přístup přes ExpressRoute. Také vysvětluje vytváření bezpečnější služeb ve virtuálních sítí Azure.

## <a name="fast-start"></a>Rychlý start
Následující graf logiku Hello můžete nastavit jste tooa konkrétní příklad hello mnoho postupů zabezpečení, k dispozici hello platformy Azure. Stručná referenční příručka najít hello příklad, který nejlépe vyhovuje váš případ. Pro rozšířené vysvětlení pokračujte v načítání prostřednictvím hello dokumentu.
[![0]][0]

[Příklad 1: Vytvoření hraniční síti (označované také jako DMZ, demilitarizovaná zóna nebo monitorovaná podsíť) toohelp ochrana aplikací pomocí skupin zabezpečení sítě (Nsg).](#example-1-build-a-perimeter-network-to-help-protect-applications-with-nsgs)</br>
[Příklad 2: Vytvoření hraniční sítě toohelp chránit aplikací s bránou firewall a skupin Nsg.](#example-2-build-a-perimeter-network-to-help-protect-applications-with-a-firewall-and-nsgs)</br>
[Příklad 3: Vytvoření hraniční sítě toohelp ochranu sítí s bránu firewall, trasy definované uživatelem (UDR) a skupina NSG.](#example-3-build-a-perimeter-network-to-help-protect-networks-with-a-firewall-and-udr-and-nsg)</br>
[Příklad 4: Přidejte hybridní připojení site-to-site, virtuální zařízení virtuální privátní síti (VPN).](#example-4-add-a-hybrid-connection-with-a-site-to-site-virtual-appliance-vpn)</br>
[Příklad 5: Přidejte hybridní připojení site-to-site, brána Azure VPN.](#example-5-add-a-hybrid-connection-with-a-site-to-site-azure-vpn-gateway)</br>
[Příklad 6: Přidejte hybridní připojení prostřednictvím ExpressRoute.](#example-6-add-a-hybrid-connection-with-expressroute)</br>
Příklady pro přidání připojení mezi virtuálními sítěmi, vysokou dostupnost a řetězení služby bude přidán toothis dokumentu přes hello další několik měsíců.

## <a name="microsoft-compliance-and-infrastructure-protection"></a>Ochrana Microsoft dodržování předpisů a infrastruktury
organizace toohelp dodržovat national, místní a a průmyslové požadavky na hello shromažďování a používání dat se jednotlivce, společnost Microsoft nabízí víc než 40 certifikace a atestace podle. Hello nejúplnější sada všechny poskytovatele cloudové služby.

Další informace najdete v tématu informace o kompatibilitě hello na hello [Microsoft Trust Center][TrustCenter].

Společnost Microsoft nemá komplexní přístup tooprotect cloudové infrastruktury potřebné toorun velkého rozsahu služeb global services. Infrastruktura cloudu Microsoft zahrnuje hardware, software, sítě a správu a provozní personál, kromě toohello fyzické datová centra.

![2]

Tento přístup poskytuje bezpečnější základ pro zákazníky toodeploy jejich služeb v cloudu Microsoft hello. dalším krokem Hello je pro zákazníky toodesign a vytvořte tooprotect architektuře zabezpečení těchto služeb.

## <a name="traditional-security-architectures-and-perimeter-networks"></a>Tradiční zabezpečení architektury a hraniční sítě
I když Microsoft investují výraznou při ochraně hello cloudové infrastruktury, musí zákazníkům taky chránit jejich cloudové služby a skupiny prostředků. Toosecurity s více vrstvami přístup poskytuje nejlepší obrany hello. Zóny zabezpečení hraniční sítě chrání prostředkům v interní síti z nedůvěryhodné sítě. Hraniční síti odkazuje toohello okraje nebo částech sítě hello umístěné mezi hello Internet a enterprise hello chráněné infrastruktury IT.

V typické podnikové sítě se hello základní infrastruktury výraznou přidá v okruhu hello s více vrstvami zabezpečení zařízení. zařízení a body vynucení zásad se skládá Hello hranic každé vrstvě. Jednotlivé úrovně může obsahovat kombinaci hello následující zařízení zabezpečení sítě: brány firewall, odmítnutí služby (DoS) prevence, zjišťování neoprávněných vniknutí nebo systémy ochrany (ID nebo IP adresy) a zařízení VPN. Vynucení zásad může trvat hello formě zásady brány firewall, seznamy řízení přístupu (ACL) nebo konkrétní směrování. Hello první linií obrany v síti hello přímo přijímat příchozí provoz z Internetu, hello je kombinací těchto mechanismů tooblock útoků a škodlivé provozu současně další oprávněné požadavky do sítě hello. Tento provoz směruje přímo tooresources hello hraniční sítě. Tento prostředek může pak "komunikovat" tooresources hlubší v síti hello tranzitní hello další hranice pro ověření první. vrstva nejkrajnější Hello se nazývá hello hraniční síti, protože tuto část hello sítě je zveřejněné toohello Internetu, obvykle s určitou formu ochrany na obou stranách. Hello následující obrázek znázorňuje příklad hraniční síti jedné podsíti v podnikové síti, s dvěma hranice zabezpečení.

![3]

Existuje mnoho architektur používá tooimplement hraniční síti. Tyto architektury můžete s různým mechanismy na každý z nich hranic tooblock přenosy v rozsahu od jednoduchého zatížení vyrovnávání tooa více podsítěmi hraniční síti a chránit hello hlubších vrstev hello podnikové síti. Jak hello hraniční síti vychází závisí na konkrétním požadavkům hello hello organizace a jeho celkový odolnost vůči rizikům.

Jak zákazníci přesunout jejich cloudy toopublic úlohy, je důležité toosupport podobné možnosti pro architekturu hraniční sítě v Azure toomeet kompatibilita a požadavky na zabezpečení. Tento dokument obsahuje pokyny na tom, jak zákazníci mohou vytvářet prostředí zabezpečené sítě v Azure. Se zaměřuje na hello hraniční síti, ale také zahrnuje komplexní diskuzi o mnoho aspektů zabezpečení sítě. Následující otázky Hello informovat toto pojednání:

* Jak se dají vytvářet do hraniční sítě v Azure?
* Jaké jsou některé z hello funkce Azure k dispozici toobuild hello hraniční síti?
* Jak lze chránit úlohy back-end?
* Jak jsou internetové komunikace řídí toohello úlohy v Azure?
* Jak se dají hello místními sítěmi chránit z nasazení v Azure?
* Pokud má být funkce nativní zabezpečení Azure použita versus zařízení třetích stran nebo služby?

Hello následující diagram znázorňuje různých úrovní zabezpečení, které Azure poskytuje toocustomers. Tyto vrstvy jsou nativní v hello Azure k samotné platformě a uživatelsky definované funkce:

![4]

Příchozí z hello Internetu, DDoS Azure pomáhá chránit proti rozsáhlé útoky na Azure. Další vrstva Hello je zákazník definovaný veřejné IP adresy (koncových bodů), které jsou používané toodetermine které přenos dat prostřednictvím hello cloudové služby toohello virtuální sítě. Nativní Azure virtuální izolace sítě zajišťuje úplný izolaci od všech ostatních sítí a že jenom přenosy dat prostřednictvím uživateli nakonfigurovanému cesty a metody. Tyto cesty a metody jsou další vrstva hello, kde skupiny Nsg, UDR a síťových virtuálních zařízení můžou být použité toocreate zabezpečení hranice tooprotect hello aplikace nasazení v síti hello chráněný.

Hello následující část obsahuje přehled virtuálních sítí Azure. Tyto virtuální sítě jsou vytvořené pomocí zákazníků a jsou co jejich nasazených úloh jsou připojené k. Virtuální sítě jsou hello základ pro všechny funkce zabezpečení sítě hello vyžaduje tooestablish hraniční síti tooprotect zákaznických nasazení v Azure.

## <a name="overview-of-azure-virtual-networks"></a>Přehled virtuálních sítí Azure
Před internetové přenosy můžete získat toohello virtuálních sítí Azure, existují dvě vrstvy zabezpečení vyplývajících toohello platformy Azure:

1.    **Ochrana proti útoku DDoS**: ochrana proti útoku DDoS je vrstvu hello Azure fyzické sítě, který chrání hello samotné platformě Azure z rozsáhlé útoky založenými na Internetu. Tyto útoky pomocí několika uzlů "robota" v pokusu o toooverwhelm služby sítě Internet. Azure má robustní mřížku DDoS ochrany na všech příchozích, odchozích a mezi Azure oblast připojení. Tuto vrstvu ochrany DDoS nemá žádné atributy konfigurovat uživatele a není přístupný toohello zákazníka. Hello DDoS ochrannou vrstvu chrání Azure jako platformu před útoky ve velkém měřítku, také sleduje odesílací vázané provozu a provoz mezi Azure oblast. Pomocí virtuálních síťových zařízení na hello virtuální síť, můžete nakonfigurovat další vrstvy odolnost proti zákazníkem hello proti menší útoku škálování, který nemá dojít hello platformy úroveň ochrany. Příklad DDoS v akci; Pokud pomocí rozsáhlé útoku DDoS byl napaden internetovou IP adresu, by Azure zjišťovat zdroje hello hello útoků a procházet nástroje hello poškozený přenosů, než bylo dosaženo plánovaného místa určení. Ve většině případů hello attacked koncový bod nemá vliv hello útoku. Ve výjimečných případech hello že koncový bod ovlivňuje žádný provoz je ovlivněných tooother koncových bodů, pouze hello napadení koncový bod. Další zákazníků a služby, proto by viz žádný vliv z tohoto útoku. Je důležité toonote, který Azure DDoS pouze hledá rozsáhlé útoky. Je možné, že může být specifické služby přetížena předtím, než se překročí mezní hodnoty úroveň ochrany platformy hello. Například webu na jednom serveru A0 IIS, může do režimu offline pomocí útoku DDoS předtím, než úroveň platformy Azure ochrana proti útoku DDoS registrována hrozbou.

2.  **Veřejné IP adresy**: veřejné IP adresy (povolené prostřednictvím koncových bodů služby, veřejné IP adresy, aplikační brána a dalších Azure funkcí, které k dispozici prostředek veřejné IP adresy toohello Internetu směrovat tooyour) umožňují cloudové služby nebo toohave veřejné internetové IP adresy a porty, které jsou zveřejněné skupiny zdrojů. koncový bod Hello používá překládání adres (NAT) tooroute provoz toohello interní adresa a port na hello virtuální síť Azure. Tato cesta je hello primární způsob toopass externích přenosů do virtuální sítě hello. Hello veřejné IP adresy jsou konfigurovatelná toodetermine, jaký provoz, je předaná a jak a kde je přeložená na toohello virtuální sítě.

Provoz dosáhne hello virtuální sítě, budete řadu funkcí, které je možné uplatnit. Virtuální sítě Azure jsou hello základ pro zákazníky tooattach jejich úlohy a kde platí základní zabezpečení na úrovni sítě. Je privátní síť (překrytí virtuální sítě) v Azure pro zákazníky s hello následující funkce a vlastnosti:

* **Izolace provozu**: virtuální sítě je hranice izolace provozu hello na hello platformy Azure. Virtuální počítače (VM) v jednu virtuální síť nemůže komunikovat přímo tooVMs v jinou virtuální síť, i když jsou obě virtuální sítě vytvořené pomocí hello tentýž zákazník. Izolace je kritické vlastnosti, které zajišťuje, aby virtuální počítače zákazníka a komunikace zůstane privátní virtuální sítě.

>[!NOTE]
>Izolace přenosů dat odkazuje jenom tootraffic *příchozí* toohello virtuální sítě. Ve výchozí odchozí provoz z virtuální sítě toohello hello Internetu je povoleno, ale je možné zabránit v případě potřeby pomocí skupin Nsg.
>
>

* **Vícevrstvá topologie**: virtuální sítě poskytují zákazníkům toodefine vícevrstvé topologie přidělením podsítě a určení samostatné adresních prostorů pro různé prvky nebo "vrstvy" jejich úloh. Tyto logické seskupení a topologie zákazníci toodefine různý přístup zásady na základě typů hello zatížení a také řídit tok přenosů dat mezi vrstvami hello.
* **Připojení mezi různými místy**: Zákazníci mohou vytvořit připojení mezi různými místy mezi virtuální síť a více místními servery nebo jiných virtuálních sítí v Azure. tooconstruct připojení zákazníci mohou používat virtuální síť partnerský vztah Azure VPN Gateway, síťových virtuálních zařízení nebo ExpressRoute. Azure podporuje sítě site-to-site (S2S) VPN pomocí standardních protokolů protokolu IPsec/IKE a privátní připojení ExpressRoute.
* **Skupina NSG** umožňuje zákazníkům toocreate pravidla (ACL) na úrovni hello potřeby členitosti: síťového rozhraní, jednotlivé virtuální počítače nebo virtuální podsítě. Zákazníci můžete řídit přístup k povolení nebo odepření komunikace mezi hello zatížení v rámci virtuální sítě, ze systémů v sítích zákazníka prostřednictvím připojení mezi různými místy, nebo přímé komunikaci v síti Internet.
* **UDR** a **předávání IP adres** zákazníkům umožňují toodefine hello komunikační trasy mezi různých vrstev v rámci virtuální sítě. Zákazníci můžete nasadit bránu firewall, ID nebo IP adresy a jiné virtuální zařízení a směrovat síťový provoz přes tyto zabezpečovací zařízení pro vynucení hranic zásad zabezpečení, auditování a kontroly.
* **Síťových virtuálních zařízení** v hello Azure Marketplace: zabezpečovací zařízení, jako jsou brány firewall, nástroje pro vyrovnávání zatížení a ID nebo IP adresy jsou dostupné v Azure Marketplace hello a hello Galerie obrázků virtuálních počítačů. Zákazníci můžou nasazovat těchto zařízení do svých virtuálních sítích a konkrétně v jejich zabezpečení hranice (včetně hello podsítě v hraniční síti) víceúrovňových toocomplete zabezpečeném síťovém prostředí.

Tyto funkce a možnosti je jeden příklad, jak může v Azure zkonstruovat síťovou architekturu hraniční hello následující diagram:

![5]

## <a name="perimeter-network-characteristics-and-requirements"></a>Hraniční síť charakteristiky a požadavky
Hello hraniční síti je front-endu hello sítě, přímo propojení komunikace z hello Internet hello. příchozí pakety Hello musí procházet skrz hello zabezpečovací zařízení, jako jsou brány firewall hello, ID a IP adresy, dříve, než dorazila hello back-end serverů. Internetový pakety z hello úloh můžete také procházet skrz hello zabezpečovací zařízení v hraniční síti hello pro vynucení zásad, kontrolu a auditování účely, před opuštěním hello sítě. Kromě toho hello hraniční síti, může hostovat brány VPN mezi různými místy mezi virtuální sítě zákazníků a místní sítě.

### <a name="perimeter-network-characteristics"></a>Hraniční síť charakteristiky
Odkazování na předchozím obrázku hello, některé charakteristiky hello dobrý hraniční sítě jsou následující:

* Internetový:
  * Hello hraniční sítě samotné podsítě je přístupný z Internetu, komunikuje přímo s hello Internetu.
  * Veřejné IP adresy, virtuální IP adresy a koncové body služby předávání zařízení a internetové přenosy toohello síť front-end.
  * Příchozí provoz z Internetu projdou zabezpečovací zařízení před dalším prostředkům v síti front-end hello hello.
  * Pokud je povoleno odchozí zabezpečení, provoz prochází přes zabezpečovací zařízení, jako poslední krok text hello, před předáním toohello Internetu.
* Chráněná síťová:
  * Neexistuje žádné přímé cestu z hello Internet toohello základní infrastruktury.
  * Kanály toohello základní infrastruktury musí procházet přes zabezpečovací zařízení, jako jsou skupiny Nsg, brány firewall nebo zařízení VPN.
  * Jiná zařízení nesmí přemostění Internetu a hello základní infrastruktury.
  * Zabezpečovací zařízení na obou hello internetového a hello chráněná síťová čelí hranice hello hraniční síti (například hello dvě brány firewall ikony ukazuje předchozí obrázek hello) ve skutečnosti může být jeden virtuální zařízení pomocí různých druhů pravidel nebo rozhraní pro každou hranici. Například jeden fyzického zařízení logicky oddělené, zpracování hello zatížení pro obě hranice hello hraniční síti.
* Dalších běžných postupů a omezení:
  * Úlohy nesmí ukládání důležitých informací business.
  * Konfigurace sítě tooperimeter přístupu a aktualizace a nasazení jsou omezené tooonly oprávnění správce.

### <a name="perimeter-network-requirements"></a>Požadavky na hraniční síť
tooenable tyto charakteristiky, postupujte podle těchto pokynů na virtuální síť požadavky tooimplement úspěšné hraniční síti:

* **Architektura podsítě:** zadejte hello virtuální sítě tak, aby celou podsíť je vyhrazený jako hello hraniční síti, oddělený od jiných podsítí ve hello stejné virtuální síti. Toto oddělení zajišťuje hello provoz mezi hello hraniční síti a jinými interní nebo privátní podsítě vrstvy toky přes bránu firewall či virtuální zařízení ID nebo IP adresy.  Trasy definované uživatelem na hello hranic, které podsítě jsou požadované tooforward tento provoz toohello virtuální zařízení.
* **Skupina NSG:** hello hraniční sítě samotné podsítě musí být otevřený tooallow komunikaci s hello Internet, ale to neznamená, zákazníci měli obcházení skupiny Nsg. Postupujte podle běžné zabezpečení postupy toominimize hello sítě povrchy zveřejněné toohello Internetu. Zamknout rozsahy Vzdálená adresa hello povolené tooaccess hello nasazení nebo hello konkrétní aplikaci protokoly a porty, které jsou otevřené. Může být okolností, ale, ve kterých není možné dokončení uzamčení. Například pokud zákazníci externí web ve službě Azure, hello hraniční síti by měl povolit hello příchozí webové žádosti z všechny veřejné IP adresy, ale měli otevírat pouze hello webové aplikace porty: TCP na portu 80 nebo TCP na portu 443.
* **Směrovací tabulky:** hello hraniční sítě samotné podsítě musí být schopný toocommunicate toohello Internetu přímo, ale neměli povolit tooand přímou komunikaci z hello back end a místní sítí bez průchodu přes bránu firewall nebo zabezpečení zařízení.
* **Konfigurace zabezpečení zařízení:** tooroute a kontrola paketů mezi hello hraniční síti a hello zbytek hello chráněné sítě, hello zabezpečovací zařízení, jako je například Brána firewall, ID, a IP adresy zařízení může být více adresami. Můžou mít oddělených síťových adaptérů pro hello hraniční sítě a podsítě hello back-end. síťové adaptéry Hello v hraniční síti hello komunikovat přímo tooand z hello Internet, s hello odpovídající skupiny Nsg a hello hraniční sítě směrovací tabulky. síťové adaptéry Hello připojení toohello back-end podsítě mají omezený více skupin Nsg a směrovacích tabulek hello odpovídající podsítě back-end.
* **Funkce zabezpečení zařízení:** hello zabezpečovací zařízení nasazený v hraniční síti hello obvykle provést hello následující funkce:
  * Brány firewall: Vynucování pravidel brány firewall nebo zásady řízení přístupu pro příchozí požadavky hello.
  * Hrozby odhalování a prevence: zjišťování a zmírnění nebezpečné útoky hello Internetu.
  * Auditování a protokolování: zachování podrobné protokoly auditování a analýz.
  * Reverse proxy: přesměrování hello příchozí požadavky toohello odpovídající back-end serverů. Toto přesměrování zahrnuje mapování a překladu hello cílové adresy na front-endu zařízení hello, obvykle brána firewall, toohello serveru back-end adresy.
  * Předávání proxy: poskytnutí NAT a provádění auditování pro komunikaci se spustily z funkce toohello hello virtuální sítě Internet.
  * Směrovač: Předávání mezi podsítěmi a příchozí provoz ve virtuální síti hello.
  * Zařízení VPN: funguje jako hello mezi různými místy brány sítě VPN pro mezi různými místy VPN připojení mezi místními sítěmi zákazníka a virtuálních sítí Azure.
  * Server sítě VPN: přijímání klienti VPN připojení tooAzure virtuální sítě.

> [!TIP]
> Zachovat hello následující dva samostatné skupiny: hello jednotlivce oprávnění tooaccess hello hraniční sítě zabezpečení zařízení a hello jednotlivce oprávnění jako správci vývoj, nasazení nebo provozu aplikace. Tyto skupiny oddělíte umožňuje oddělení povinností a zabrání jedna osoba obcházení zabezpečení aplikací a ovládací prvky zabezpečení sítě.
>
>

### <a name="questions-toobe-asked-when-building-network-boundaries"></a>Otázky toobe výzva při sestavování hranice sítě
V této části, pokud není výslovně uvedeno, hello termín "sítě" odkazuje tooprivate Azure virtuální sítě vytvořené správcem předplatného. termín Hello neodkazuje toohello základní fyzické sítě v rámci Azure.

Navíc virtuálních sítí Azure jsou často používané tooextend tradiční místní sítě. Je možné tooincorporate site-to-site nebo ExpressRoute hybridní řešení pomocí architektury sítě hraniční sítě. Tento odkaz hybridní je důležitý faktor při vytváření hranic zabezpečení sítě.

Hello tyto tři otázky, které jsou kritické tooanswer když sestavujete síť s hraniční sítí a více hranic zabezpečení.

#### <a name="1-how-many-boundaries-are-needed"></a>1) hranice kolik je potřeba?
první bod rozhodnutí Hello je toodecide kolik hranice zabezpečení jsou potřeba v této situaci:

* Jednoho hraničního: jednu na hello front-end hraniční síti, mezi hello virtuální síť a hello Internet.
* Dva hranice: jeden na hello straně Internetu hello hraniční síti a druhý mezi podsíť hello hraniční sítě a podsítě hello back-end v hello virtuálních sítí Azure.
* Tři hranice: jeden na straně Internetu hello hello hraniční síti, jeden mezi hello hraniční sítě a podsítě back-end a jeden mezi podsítěmi hello back-end a hello do místní sítě.
* Hranice N: Číslo proměnné. V závislosti na požadavcích zabezpečení neexistuje žádné omezení toohello počet hranic zabezpečení, které mohou být použity v dané síti.

Hello počet a typ hranice potřeby lišit na základě společnosti riziko proti chybám a hello konkrétní scénář se implementuje. Toto rozhodnutí se často provádí společně s více skupin v rámci organizace, často včetně team rizika a dodržování předpisů, sítě a platformy týmu a vývojovým týmem aplikace. Osoby s znalosti o zabezpečení, hello data související se situací a používá technologie hello měli indikované toto rozhodnutí tooensure hello příslušná bezpečnostní potřebujete pro každou implementaci.

> [!TIP]
> Použijte hello nejmenší počet hranic, které splňují požadavky na zabezpečení hello pro dané situaci. S další hranice operace a řešení potíží s může být obtížnější, jakož i hello správu režie nezbytné při správě hello více hranic zásad v čase. Nedostatečná hranice však zvýšit tak riziko. Hledání hello vyrovnávání je velmi důležité.
>
>

![6]

Hello předchozí obrázek zobrazuje souhrnné zobrazení tři zabezpečení hraniční sítě. Hello hranice jsou mezi hello hraniční síti a hello Internet, hello Azure front-end a back-end privátní podsítě a hello Azure podsítě back-end a hello místní podnikové síti.

#### <a name="2-where-are-hello-boundaries-located"></a>2), kde se nachází hello hranice?
Jakmile se rozhodli hello počet hranic, kde je je tooimplement hello dalším kritériem při rozhodování. Existují obecně tři možnosti:

* Pomocí internetové zprostředkující služby (například cloudové brány firewall webových aplikací, který není popisovaný v tomto dokumentu)
* Pomocí nativní funkce nebo síťových virtuálních zařízení v Azure
* Pomocí fyzických zařízení v místní síti hello

Na výhradně Azure sítě jsou možnosti hello nativní funkce Azure (například Azure nástroje pro vyrovnávání zatížení) nebo síťových virtuálních zařízení z hello ekosystém bohaté partnera Azure (například kontrolní bod brány firewall).

Pro potřeby hranice mezi Azure a místní sítí hello zabezpečovací zařízení mohou být uloženy na obou stranách připojení hello (nebo na obou stranách). Proto musí být provedeny rozhodnutí na hello umístění tooplace zabezpečení zařízení.

Na předchozím obrázku hello hello Internetu do hraniční sítě a hello přední na back-end hranice jsou zcela obsažen v rámci Azure a musí být nativní funkce Azure nebo virtuální síťové zařízení. Zabezpečovací zařízení na hello hranice mezi Azure (back-end podsítě) a hello podnikové síti může být buď na hello Azure straně nebo straně hello místní nebo i kombinaci zařízení na obou stranách. Může být významné výhody a nevýhody tooeither možnost, která je třeba zvážit vážně.

Například pomocí stávajících zařízeních fyzického zabezpečení na hello místní sítě straně má hello výhodu, že je potřeba žádné nové zařízení. Potřebuje pouze změny konfigurace. Nevýhodou Hello je však, že veškerý provoz musí vraťte z Azure toohello místní sítě toobe pohledu hello zabezpečení zařízení. Proto Azure do Azure provoz může způsobit významné latence a mít vliv na výkon a uživatelské prostředí aplikace, pokud se vynutilo back toohello místní sítě pro vynucení zásad zabezpečení.

#### <a name="3-how-are-hello-boundaries-implemented"></a>3), jak jsou implementované hello hranice?
Každá hranice zabezpečení bude pravděpodobně mít požadavků na různé možnosti (například ID a pravidla brány firewall na straně Internetu hello hraniční síti, ale jenom na seznamy ACL mezi hello hraniční síť a podsíť back-end hello). Rozhodování, na které zařízení (nebo kolik zařízení) toouse závisí na hello scénář a bezpečnostní požadavky. V následující části hello příklady 1, 2 a 3 popisují některé možnosti, které by bylo možné použít. Kontrola funkce hello nativní síť Azure a hello zařízení v Azure k dispozici z ekosystém partnerů hello ukazuje hello řadu možností k dispozici toosolve téměř každý scénář.

Dalším kritériem při rozhodování klíče implementace je jak tooconnect hello místní síť s Azure. Můžete použít hello Azure Brána virtuální nebo virtuální síťové zařízení? Tyto možnosti jsou popsány podrobněji na hello následující části (příklady 4, 5 a 6).

Kromě toho může být potřeba provoz mezi virtuálními sítěmi v rámci Azure. Tyto scénáře bude přidána v budoucí hello.

Jakmile znáte odpovědi hello toohello předchozí dotazy, hello [rychlého spuštění](#fast-start) část vám pomůže zjistit, které příkladů nejlépe vyhovuje danému scénáři.

## <a name="examples-building-security-boundaries-with-azure-virtual-networks"></a>Příklady: Vytváření hranic zabezpečení s virtuální sítě Azure
### <a name="example-1-build-a-perimeter-network-toohelp-protect-applications-with-nsgs"></a>Příklad 1 sestavení hraniční sítě toohelp chránit aplikace pomocí skupin Nsg
[Zpět tooFast start](#fast-start) | [podrobné pokyny v tomto příkladu pro sestavení][Example1]

[![7]][7]

#### <a name="environment-description"></a>Popis prostředí
V tomto příkladu je předplatné, které obsahuje hello následující prostředky:

- Jedna skupina prostředků
- Virtuální síť se dvěma podsítěmi: "FrontEnd" a "Back-end"
- Skupinu zabezpečení sítě, která je použitá tooboth podsítě
- Windows server, který představuje server webových aplikací ("IIS01")
- Dva servery Windows, které představují servery back-end aplikace ("AppVM01", "AppVM02")
- Windows server, který představuje server DNS ("DNS01")
- Veřejné IP adresy přidružené k serveru webové aplikace hello

Skripty a šablonu Azure Resource Manager najdete v tématu hello [podrobné pokyny pro sestavení][Example1].

#### <a name="nsg-description"></a>Popis skupiny NSG
V tomto příkladu je skupinu NSG vytvořené a pak načtená šesti pravidla.

> [!TIP]
> Obecně řečeno, měli byste vytvořit konkrétní pravidel "Povolit" nejprve následuje hello více obecné "Deny" pravidla. Hello Priorita určuje, která pravidla se vyhodnocují jako první. Jakmile provoz nenajde tooapply tooa konkrétní pravidlo, jsou vyhodnotit žádná další pravidla. Pravidla NSG můžete použít na buď hello příchozí nebo odchozí směr (z hlediska hello hello podsítě).
>
>

Deklarativně jsou pro příchozí provoz sestavuje hello následující pravidla:

1. Interní DNS provoz (port 53) je povolený.
2. Provoz protokolu RDP (portu 3389) z Internetu tooany hello virtuálního počítače je povolena.
3. Je povolen přenos HTTP (port 80) ze serveru tooweb Internet hello (IIS01).
4. Jakýkoli přenos (všechny porty) z IIS01 tooAppVM1 je povolen.
5. Jakýkoli přenos (všechny porty) z hello Internet toohello celý virtuální sítě (obě podsítě) byl odepřen.
6. Jakýkoli přenos (všechny porty) z hello front-end podsíť toohello back-end podsítě byl odepřen.

Pomocí těchto pravidel vázané tooeach podsíti, pokud požadavek HTTP byl příchozí z hello Internet toohello webový server, obě pravidla 3 (Povolit) a 5 (Odepřít) by použít. Ale vzhledem k tomu, že pravidlo 3 má vyšší prioritu, pouze by použít, a pravidla 5 nebude možné uplatnit. Proto hello požadavku HTTP bude mít možnost toohello webový server. Pokud tento stejný provoz pokoušel server hello DNS01 tooreach, pravidla 5 (Odepřít) by hello první tooapply a provoz hello nebude povolen toopass toohello serveru. Pravidlo 6 (Odepřít) bloky hello front-end podsíť z rozhovoru toohello back-end podsítě (s výjimkou povolené přenosy v pravidlech 1 a 4). Této sady pravidel chrání síť back-end hello v případě, že útočník ohrožuje hello webové aplikace na front-endu hello. Hello útočník by mít omezený přístup toohello "chráněné" síť back-end (pouze tooresources zveřejněné na serveru AppVM01 hello).

Je výchozí odchozí pravidlo, které umožňuje provozu toohello Internetu. V tomto příkladu jsme se umožňuje odchozí provoz a úprava není žádná odchozí pravidla. toolock dolů provoz v obou směrech, uživatelem definované směrování je vyžadován (viz příklad 3).

#### <a name="conclusion"></a>Závěr
V tomto příkladu je relativně jednoduché a snadný způsob izolace hello back-end podsíť z příchozí provoz. Další informace najdete v tématu hello [podrobné pokyny pro sestavení][Example1]. Tyto pokyny obsahují:

* Jak toobuild tato hraniční sítě pomocí klasického skriptů prostředí PowerShell.
* Jak toobuild tato hraniční sítě pomocí šablony Azure Resource Manager.
* Podrobný popis každého příkazu NSG.
* Scénáře podrobné provoz tok zobrazuje jak povolené nebo zakázané v každé vrstvě přenosy.


### <a name="example-2-build-a-perimeter-network-toohelp-protect-applications-with-a-firewall-and-nsgs"></a>Příklad 2 sestavení hraniční sítě toohelp chránit aplikací s bránou firewall a skupiny Nsg
[Zpět tooFast start](#fast-start) | [podrobné pokyny v tomto příkladu pro sestavení][Example2]

[![8]][8]

#### <a name="environment-description"></a>Popis prostředí
V tomto příkladu je předplatné, které obsahuje hello následující prostředky:

* Jedna skupina prostředků
* Virtuální síť se dvěma podsítěmi: "FrontEnd" a "Back-end"
* Skupinu zabezpečení sítě, která je použitá tooboth podsítě
* Virtuální zařízení sítě, v tomto případě a brány firewall, připojení front-end podsíť toohello
* Windows server, který představuje server webových aplikací ("IIS01")
* Dva servery Windows, které představují servery back-end aplikace ("AppVM01", "AppVM02")
* Windows server, který představuje server DNS ("DNS01")

Skripty a šablonu Azure Resource Manager najdete v tématu hello [podrobné pokyny pro sestavení][Example2].

#### <a name="nsg-description"></a>Popis skupiny NSG
V tomto příkladu je skupinu NSG vytvořené a pak načtená šesti pravidla.

> [!TIP]
> Obecně řečeno, měli byste vytvořit konkrétní pravidel "Povolit" nejprve následuje hello více obecné "Deny" pravidla. Hello Priorita určuje, která pravidla se vyhodnocují jako první. Jakmile provoz nenajde tooapply tooa konkrétní pravidlo, jsou vyhodnotit žádná další pravidla. Pravidla NSG můžete použít na buď hello příchozí nebo odchozí směr (z hlediska hello hello podsítě).
>
>

Deklarativně jsou pro příchozí provoz sestavuje hello následující pravidla:

1. Interní DNS provoz (port 53) je povolený.
2. Provoz protokolu RDP (portu 3389) z Internetu tooany hello virtuálního počítače je povolena.
3. Všechny internetové přenosy (všechny porty) toohello sítě virtuální zařízení (brány firewall) je povolen.
4. Jakýkoli přenos (všechny porty) z IIS01 tooAppVM1 je povolen.
5. Jakýkoli přenos (všechny porty) z hello Internet toohello celý virtuální sítě (obě podsítě) byl odepřen.
6. Jakýkoli přenos (všechny porty) z hello front-end podsíť toohello back-end podsítě byl odepřen.

Pomocí těchto pravidel vázané tooeach podsíti, pokud požadavek HTTP byl příchozí z hello internetovou toohello bránu firewall, obě pravidla 3 (Povolit) a 5 (Odepřít) by použít. Ale vzhledem k tomu, že pravidlo 3 má vyšší prioritu, pouze by použít, a pravidla 5 nebude možné uplatnit. Proto hello HTTP požadavku bude mít možnost toohello brány firewall. Pokud tento stejný provoz snažil tooreach hello IIS01 serveru, i když je v podsíti hello front-endu, pravidla 5 (Odepřít) by použít, a provoz hello nebude povolen toopass toohello serveru. Pravidlo 6 (Odepřít) bloky hello front-end podsíť z rozhovoru toohello back-end podsítě (s výjimkou povolené přenosy v pravidlech 1 a 4). Této sady pravidel chrání síť back-end hello v případě, že útočník ohrožuje hello webové aplikace na front-endu hello. Hello útočník by mít omezený přístup toohello "chráněné" síť back-end (pouze tooresources zveřejněné na serveru AppVM01 hello).

Je výchozí odchozí pravidlo, které umožňuje provozu toohello Internetu. V tomto příkladu jsme se umožňuje odchozí provoz a úprava není žádná odchozí pravidla. toolock dolů provoz v obou směrech, uživatelem definované směrování je vyžadován (viz příklad 3).

#### <a name="firewall-rule-description"></a>Popis pravidla brány firewall
V bráně firewall hello by se vytvořit pravidla předávání. Pravidlo je potřeba, protože tento příklad pouze směrování provozu v internetový toohello brány firewall a pak toohello webový server, pouze jedna předávání překlad síťových adres (NAT).

Hello předávání pravidlo přijme všechny příchozí zdroj adresu, která dotkne brány firewall hello pokusu o tooreach protokolu HTTP (port 80 nebo 443 pro protokol HTTPS). Má odeslaných z místního rozhraní hello brány firewall a přesměrování toohello webového serveru pomocí hello 10.0.1.5 IP adresu.

#### <a name="conclusion"></a>Závěr
V tomto příkladu je poměrně snadný způsob, jak ochraně vaší aplikace s bránou firewall a izolace hello back-end podsíť z příchozí přenosy. Další informace najdete v tématu hello [podrobné pokyny pro sestavení][Example2]. Tyto pokyny obsahují:

* Jak toobuild tato hraniční sítě pomocí klasického skriptů prostředí PowerShell.
* Jak toobuild v tomto příkladu pomocí šablony Azure Resource Manager.
* Podrobný popis každého příkazu a brány firewall pravidla NSG.
* Scénáře podrobné provoz tok zobrazuje jak povolené nebo zakázané v každé vrstvě přenosy.

### <a name="example-3-build-a-perimeter-network-toohelp-protect-networks-with-a-firewall-and-udr-and-nsg"></a>Příklad 3 sestavení hraniční sítě toohelp ochranu sítí s použitím brány firewall a UDR a NSG
[Zpět tooFast start](#fast-start) | [podrobné pokyny v tomto příkladu pro sestavení][Example3]

[![9]][9]

#### <a name="environment-description"></a>Popis prostředí
V tomto příkladu je předplatné, které obsahuje hello následující prostředky:

* Jedna skupina prostředků
* Virtuální síť s tři podsítě: "SecNet", "FrontEnd" a "Back-end"
* Virtuální zařízení sítě, v tomto případě a brány firewall, připojení toohello SecNet podsítě
* Windows server, který představuje server webových aplikací ("IIS01")
* Dva servery Windows, které představují servery back-end aplikace ("AppVM01", "AppVM02")
* Windows server, který představuje server DNS ("DNS01")

Skripty a šablonu Azure Resource Manager najdete v tématu hello [podrobné pokyny pro sestavení][Example3].

#### <a name="udr-description"></a>Popis UDR
Ve výchozím nastavení jsou hello následující systémové trasy definované jako:

        Effective routes :
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

Hello VNETLocal je vždy jeden nebo více definovaných adresních předpon, které tvoří hello virtuální sítě pro tuto konkrétní síť (to znamená, změní ze sítě toovirtual virtuální sítě, v závislosti na tom, jak je definována každou konkrétní virtuální síť). systémové trasy zbývající Hello statické a výchozí jako uvedené v tabulce hello.

V tomto příkladu dvou směrovacích tabulek jsou vytvořen, jeden pro podsítě hello front-end a back-end. Každá tabulka je načtena s statické trasy, které jsou vhodné pro danou podsíť hello. V tomto příkladu každá tabulka měla tři tras, které směrovat všechen provoz (0.0.0.0/0) přes bránu firewall hello (dalšího směrování = virtuální zařízení IP adresa):

1. Provozu místních podsítí s žádné další směrování definované tooallow místní podsíti provozu toobypass hello brány firewall.
2. Virtuální síťový provoz s další směrování definovat jako bránu firewall. Tato další segment přepíše hello výchozí pravidlo, které umožňuje místní virtuální síť tooroute provoz přímo.
3. Veškerý zbývající provoz (0/0) s další směrování definován jako hello brány firewall.

> [!TIP]
> V hello UDR zalomení místní podsíti komunikace nemá vstupní hello místní podsíti.
>
> * V našem příkladu je důležité 10.0.1.0/24 odkazující tooVNETLocal! Bez toho paketu ponechat hello Webový Server (10.0.1.4) určené tooanother místní server (například) 10.0.1.25 selžou, protože budou odeslány toohello hodnocení chyb zabezpečení. Hello hodnocení chyb zabezpečení sítě bude odesílat toohello podsíť a podsíť hello se ji odešlete toohello hodnocení chyb zabezpečení v nekonečné smyčce.
> * Hello pravděpodobnost smyčka směrování jsou obvykle vyšší na zařízení s více síťovými adaptéry, které jsou připojené tooseparate podsítě, které je často tradiční, místní zařízení.
>
>

Po vytvoření hello směrovacích tabulek jsou musí být vázané tootheir podsítě. Hello front-end podsítě, směrovací tabulce, po vytvoření a vázán toohello podsíť, bude vypadat tento výstup:

        Effective routes :
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active

> [!NOTE]
> UDR teď může být podsíť brány použité toohello, na které hello ExpressRoute je připojen okruh.
>
> Příklady jak tooenable vaší hraniční sítě s ExpressRoute nebo sítě site-to-site jsou uvedeny v příkladech 3 a 4.
>
>

#### <a name="ip-forwarding-description"></a>Popis předávání IP
Předávání IP adres je tooUDR doprovodné funkce. Předávání IP adres je nastavení na virtuální zařízení, která to umožňuje tooreceive dat adresovaných není konkrétně toohello zařízení a pak předávání této cílové ultimate tooits provoz.

Například pokud AppVM01 provede požadavek toohello DNS01 serveru, by UDR směrovat tato brána firewall toohello provoz. U povoleno předávání IP je hello provoz pro cíl DNS01 hello (10.0.2.4) akceptovat hello zařízení (10.0.0.4) a, předá tooits ultimate cílové (10.0.2.4). Bez zapnuta brána firewall hello předávání IP adres by se provoz přijímat hello zařízení Přestože hello směrovací tabulka má hello brány firewall jako další segment hello. toouse virtuální zařízení, je důležité tooremember tooenable IP předávání společně s UDR.

#### <a name="nsg-description"></a>Popis skupiny NSG
Skupinu NSG v tomto příkladu je vytvořen a pak načten s jedním pravidlem. Tato skupina je pak vázaný jenom toohello front-end a back-end podsítě (ne hello SecNet). Deklarativně se sestavuje hello následující pravidlo:

* Jakýkoli přenos (všechny porty) z hello Internet toohello celý virtuální sítě (všechny podsítě) byl odepřen.

I když v tomto příkladu se používají skupiny Nsg, jeho hlavním účelem je jako vrstva sekundární ochranu proti ruční chybné konfigurace. Hello cílem je tooblock veškerý příchozí provoz z Internetu tooeither hello hello front-end nebo back-end podsítí. Má jenom přenos přes bránu firewall toohello hello SecNet podsítě (a pak v případě potřeby v podsítích toohello front-end nebo back-end). Plus s pravidly UDR hello v místě, přenosy, který do hello front-end nebo back-end podsítí by přesměrováni na toohello brány firewall (Děkujeme tooUDR). brány firewall Hello tento provoz může vypadat jako asymetrický toku a by vyřadit hello odchozí provoz. Proto jsou tři vrstvy zabezpečení, ochraně hello podsítě:

* Žádné veřejné IP adresy na všech front-end nebo back-end síťové adaptéry.
* Skupiny Nsg odmítání hello internetové přenosy.
* Hello brány firewall vyřazování asymetrické provoz.

Jeden bod zajímavé týkající se hello NSG v tomto příkladu je, že obsahuje pouze jeden pravidlo, které je toodeny internetové přenosy toohello celý virtuální síť, včetně podsítě zabezpečení hello. Ale vzhledem k tomu, že hello NSG je pouze vázán toohello front-end a back-end podsítě, hello pravidlo není zpracován na provoz příchozí toohello zabezpečení podsítě. V důsledku toho přenosy toohello zabezpečení podsítě.

#### <a name="firewall-rules"></a>Pravidla brány firewall
V bráně firewall hello by se vytvořit pravidla předávání. Vzhledem k tomu, že brána firewall hello je blokování nebo předávání všechny vstupní, výstupní a přenosy uvnitř virtuální sítě, je potřeba řada pravidla brány firewall. Navíc veškerý příchozí provoz dotkne hello služby zabezpečení veřejnou IP adresu (v jiné porty), toobe zpracuje bránou hello firewall. Osvědčeným postupem je toodiagram hello logické toky před nastavením hello podsítě a pravidla brány firewall, tooavoid přepracování později. Hello následující obrázek je logickém zobrazení hello pravidla brány firewall v tomto příkladu:

![10]

> [!NOTE]
> Podle hello používá virtuální síťové zařízení, hello správu porty liší. V tomto příkladu je odkazováno Barracuda NextGen Firewall, porty 22, 801 a 807, který používá. Poraďte se se hello zařízení dodavatele dokumentaci toofind hello přesný porty používané pro správu zařízení hello používá.
>
>

#### <a name="firewall-rules-description"></a>Popis pravidla brány firewall
V předchozích Logický diagram hello není podsítě zabezpečení hello zobrazit, protože brána firewall hello je hello pouze prostředků na této podsíti. Hello diagram se zobrazuje hello pravidla brány firewall a jak se logicky povolit nebo odepřít tok přenosů dat, není hello skutečné směrované cesty. Navíc hello externí porty vybraný pro hello provoz protokolu RDP jsou porty vyšší pohyboval (8014 – 8026) a byly vybrané tooloosely zarovnané s hello poslední dva oktety hello místní IP adresu pro snazší čitelnost (například místní server adresa 10.0.1.4 přidružena s portem externí 8014). Žádné vyšší porty konfliktní, ale může použít.

V tomto příkladu potřebujeme sedm typů pravidel:

* Externí pravidla (pro příchozí provoz):
  1. Pravidlo brány firewall správy: Tato aplikace přesměrování pravidlo umožňuje provoz toopass toohello správu porty hello síťové virtuální zařízení.
  2. Pravidla protokolu RDP (pro každý server systému Windows): tyto čtyři pravidla (jeden pro každý server), povolit správu hello jednotlivé servery prostřednictvím protokolu RDP. čtyři pravidla RDP Hello může také sbaleny do jednoho pravidla, v závislosti na možnosti hello hello sítě virtuální zařízení používá.
  3. Pravidla pro provoz aplikace: existují dva z těchto pravidel, hello nejprve pro hello front-end webový provoz a hello druhý pro přenosy hello back-end (například webový server toodata vrstva). Konfigurace Hello pravidla závisí na hello síťovou architekturu (kde jsou umístěné vaše servery) a tok přenosů dat (které směr hello přenosy dat a používaných portů).
     * první pravidlo Hello umožňuje hello skutečné aplikace provoz tooreach hello aplikačnímu serveru. Při hello ostatní pravidla povolit pro zabezpečení a správu, jsou pravidla pro provoz aplikace, povolit externích uživatelů nebo služeb tooaccess aplikace hello. V tomto příkladu je jednom webovém serveru na portu 80. Proto jediné aplikace pravidlo firewallu přesměruje příchozí provoz toohello externí IP, toohello webové servery interní IP adresu. Hello přesměrování provozu relace by byl přeložen prostřednictvím NAT toohello interního serveru.
     * pravidlo druhého Hello je hello back-end pravidlo tooallow hello webový server tootalk toohello AppVM01 server (ale ne AppVM02) prostřednictvím libovolný port.
* Vnitřní pravidla (pro přenosy uvnitř virtuální sítě.)
  1. Pravidlo odchozí tooInternet: Toto pravidlo umožňuje přenosy z jakékoli sítě toopass toohello vybrané sítě. Toto pravidlo je obvykle výchozí pravidlo už v bráně firewall hello, ale v zakázaném stavu. Toto pravidlo by měly být povoleny v tomto příkladu.
  2. Pravidlo DNS: Toto pravidlo umožňuje pouze (port 53) provoz toopass toohello DNS server DNS. Pro toto prostředí je blokována většina provoz z hello front-endu toohello back-end. Toto pravidlo umožňuje konkrétně DNS z jakékoli místní podsítě.
  3. Pravidlo toosubnet podsítě: Toto pravidlo je tooallow jakýkoli server na hello podsítě back-end tooconnect tooany serveru na front-endu podsíť hello (ale ne hello zpětné).
* Pohotovostního pravidlo (pro provoz, který nesplňuje některé z předchozích hello):
  1. Všechny přenosy pravidlo odepřít: Toto pravidlo Odepřít by měl být vždy hello konečné pravidlo (z hlediska priorita), a jako takový Pokud přenosový tok selže toomatch některé z předchozích pravidel hello přestane se tímto pravidlem. Toto pravidlo je výchozí pravidlo a obvykle na místě a aktivní. Žádné úpravy jsou obvykle potřebné toothis pravidlo.

> [!TIP]
> Na hello druhý aplikace provozu pravidlo, toosimplify v tomto příkladu je libovolný port povolena. Ve scénáři skutečné hello nejvíce konkrétní port a rozsahy adres by měl být použité tooreduce hello prostor pro útok toto pravidlo.
>
>

Po vytvoření pravidel předchozí hello je důležité tooreview hello prioritu každé pravidlo tooensure provozu povolený nebo zakázaný podle potřeby. V tomto příkladu jsou hello pravidla v pořadí podle priority.

#### <a name="conclusion"></a>Závěr
V tomto příkladu je složitější ale dokončit způsob ochrany a izolování hello sítě než hello předchozích příkladech. (Příklad 2 chrání pouze hello aplikace a příklad 1 právě izoluje podsítě). Tento návrh umožňuje monitorování provoz v obou směrech a chrání nejen hello příchozí aplikační server, ale vynucuje zásady zabezpečení sítě pro všechny servery v této síti. Také v závislosti na zařízení hello používá, sledování a auditování úplný provoz lze dosáhnout. Další informace najdete v tématu hello [podrobné pokyny pro sestavení][Example3]. Tyto pokyny obsahují:

* Jak toobuild tento příklad hraniční sítě pomocí klasického skriptů prostředí PowerShell.
* Jak toobuild v tomto příkladu pomocí šablony Azure Resource Manager.
* Podrobný popis jednotlivých UDR NSG příkaz a pravidla brány firewall.
* Scénáře podrobné provoz tok zobrazuje jak povolené nebo zakázané v každé vrstvě přenosy.

### <a name="example-4-add-a-hybrid-connection-with-a-site-to-site-virtual-appliance-vpn"></a>Příklad 4 přidat hybridní připojení s site-to-site, virtuální zařízení VPN
[Zpět tooFast počáteční](#fast-start) | Podrobné pokyny sestavení k dispozici co nejdříve

[![11]][11]

#### <a name="environment-description"></a>Popis prostředí
Hybridní sítě pomocí virtuální síťové zařízení (hodnocení chyb zabezpečení), mohou být přidány tooany hello hraniční sítě typu popsané v příkladech 1, 2 nebo 3.

Jak ukazuje předchozí obrázek hello, je připojení k síti VPN přes Internet (site-to-site) hello tooconnect použité místní síti tooan virtuální síť Azure prostřednictvím hodnocení chyb zabezpečení.

> [!NOTE]
> Pokud používáte ExpressRoute s hello veřejný partnerský vztah Azure možnost povolit, by se vytvořit statickou trasu. Tato statického směrování by měl směrovat toohello hodnocení chyb zabezpečení sítě VPN IP adresu mimo podnikovou síť intranet a nikoli prostřednictvím hello připojení ExpressRoute. Hello NAT na hello ExpressRoute Azure veřejného partnerského vztahu možnost vyžaduje může dojít k narušení hello VPN relace.
>
>

Po hello VPN je na místě, hello hodnocení chyb zabezpečení stane hello centrální rozbočovač pro všechny sítě a podsítě. pravidla brány firewall předávání Hello zjistěte, jaký provoz toky jsou povoleny, jsou převedeny přes NAT, zprávy jsou přesměrovány nebo zahozených (i pro toky přenosů dat mezi hello místní sítí a Azure).

Přenosové toky považovat za pečlivě, jak lze optimalizovat a snížení podle tohoto vzoru návrhu v závislosti na konkrétní hello případ použití.

Použití prostředí hello součástí příklad 3 a následným přidáním připojení site-to-site VPN hybridní sítě vytváří hello následující návrhu:

[![12]][12]

Hello místní směrovač nebo jiné zařízení sítě, který je kompatibilní s vaší hodnocení chyb zabezpečení sítě pro síť VPN, bude klient VPN hello. Toto fyzické zařízení zodpovídá za inicializaci a udržování připojení VPN typu hello vaše hodnocení chyb zabezpečení.

Logicky toohello hodnocení chyb zabezpečení sítě hello vypadá čtyři samostatné "zóny zabezpečení" pomocí pravidel hello na hello hodnocení chyb zabezpečení sítě se hello primární ředitel provozu mezi tyto zóny:

![13]

#### <a name="conclusion"></a>Závěr
Přidání Hello site-to-site VPN hybridní síťové připojení tooan virtuální síť Azure můžete rozšířit hello do místní sítě do Azure zabezpečeným způsobem. Pomocí připojení k síti VPN, provozu je šifrovaný a směruje prostřednictvím hello Internetu. Hello hodnocení chyb zabezpečení sítě v tomto příkladu poskytuje centrální umístění tooenforce a spravovat zásady zabezpečení hello. Další informace najdete v tématu hello podrobné pokyny (řádně doloženo) pro vytvoření. Tyto pokyny obsahují:

* Jak toobuild tento příklad hraniční sítě pomocí skriptů prostředí PowerShell.
* Jak toobuild v tomto příkladu pomocí šablony Azure Resource Manager.
* Scénáře podrobné provoz tok zobrazuje tok provozu prostřednictvím tohoto návrhu.

### <a name="example-5-add-a-hybrid-connection-with-a-site-to-site-azure-vpn-gateway"></a>Příklad 5 přidat hybridní připojení s brány Azure VPN site-to-site,
[Zpět tooFast počáteční](#fast-start) | Podrobné pokyny sestavení k dispozici co nejdříve

[![14]][14]

#### <a name="environment-description"></a>Popis prostředí
Hybridní sítě pomocí služby Azure VPN gateway se dá přidat tooeither hraniční sítě typu popsané v příkladech 1 nebo 2.

Jak je znázorněno v předchozích obrázek hello, je připojení k síti VPN přes Internet (site-to-site) hello tooconnect použité místní síti tooan virtuální síť Azure pomocí služby Azure VPN gateway.

> [!NOTE]
> Pokud používáte ExpressRoute s hello veřejný partnerský vztah Azure možnost povolit, by se vytvořit statickou trasu. Tato statického směrování by měl směrovat toohello hodnocení chyb zabezpečení sítě VPN IP adresu mimo podnikovou síť intranet a nikoli prostřednictvím sítě WAN ExpressRoute hello. Hello NAT na hello ExpressRoute Azure veřejného partnerského vztahu možnost vyžaduje může dojít k narušení hello VPN relace.
>
>

Hello následující obrázek znázorňuje hello dva okraje sítě v tomto příkladu. Na první edge hello hello hodnocení chyb zabezpečení a skupiny Nsg řídit tok přenosů dat uvnitř Azure sítích a mezi Azure a hello Internetu. Druhý okraj Hello je hello Azure VPN gateway, která je hraniční samostatné izolovanou síť mezi místními a Azure.

Přenosové toky považovat za pečlivě, jak lze optimalizovat a snížení podle tohoto vzoru návrhu v závislosti na konkrétní hello případ použití.

Použití prostředí hello vytvořené v příkladu 1 a následným přidáním připojení site-to-site VPN hybridní sítě vytváří hello následující návrhu:

[![15]][15]

#### <a name="conclusion"></a>Závěr
Přidání Hello site-to-site VPN hybridní síťové připojení tooan virtuální síť Azure můžete rozšířit hello do místní sítě do Azure zabezpečeným způsobem. Pomocí hello nativní Azure VPN gateway, provozu je šifrovanými v protokolu IPSec a směruje se přes hello Internet. Pomocí brány Azure VPN hello navíc nabízejí nižší náklady možnost (žádné další licence náklady stejně jako u jiných výrobců NVAs). Tato možnost je nejekonomičtější v příkladu 1, kde se používá žádné hodnocení chyb zabezpečení. Další informace najdete v tématu hello podrobné pokyny (řádně doloženo) pro vytvoření. Tyto pokyny obsahují:

* Jak toobuild tento příklad hraniční sítě pomocí skriptů prostředí PowerShell.
* Jak toobuild v tomto příkladu pomocí šablony Azure Resource Manager.
* Scénáře podrobné provoz tok zobrazuje tok provozu prostřednictvím tohoto návrhu.

### <a name="example-6-add-a-hybrid-connection-with-expressroute"></a>Příklad 6 přidat hybridní připojení s ExpressRoute
[Zpět tooFast počáteční](#fast-start) | Podrobné pokyny sestavení k dispozici co nejdříve

[![16]][16]

#### <a name="environment-description"></a>Popis prostředí
Hybridní sítě pomocí ExpressRoute soukromého partnerského vztahu připojení lze přidat tooeither hraniční sítě typu popsané v příkladech 1 nebo 2.

Jak je znázorněno v předchozích obrázek hello, soukromý partnerský vztah ExpressRoute poskytuje přímé připojení mezi místní sítí a hello virtuální síť Azure. Provoz tranzit pouze hello síti poskytovatele služeb a sítě hello Microsoft Azure, nikdy dotykové ovládání hello Internetu.

> [!TIP]
> Vypnout hello Internet pomocí služby ExpressRoute udržuje podnikové síťové přenosy. Umožňuje také pro smlouvy o úrovni služeb od poskytovatele ExpressRoute. Hello brány Azure můžete předat až GB/s too10 s ExpressRoute, zatímco se site-to-site VPN, je maximální propustnost brány Azure hello 200 MB/s.
>
>

Jak je vidět v následujícím diagramu hello, se tato možnost hello prostředí teď má dva okraje sítě. Hello hodnocení chyb zabezpečení a NSG řízení přenosové toky uvnitř Azure sítích a mezi Azure a hello Internetu, zatímco hello brány je okraj samostatné izolovanou síť mezi místními a Azure.

Přenosové toky považovat za pečlivě, jak lze optimalizovat a snížení podle tohoto vzoru návrhu v závislosti na konkrétní hello případ použití.

Použití prostředí hello vytvořené v příkladu 1 a následným přidáním ExpressRoute hybridní síťového připojení, vytváří hello následující návrhu:

[![17]][17]

#### <a name="conclusion"></a>Závěr
Přidání Hello síťovým připojením ExpressRoute privátní partnerský vztah můžete rozšířit hello do místní sítě do Azure v zabezpečené, nižší latenci a vyšší provádění způsobem. Navíc pomocí hello nativní Azure Gateway jako v následujícím příkladě nabízí možnost nižší náklady (ne další licencování stejně jako u jiných výrobců NVAs). Další informace najdete v tématu hello podrobné pokyny (řádně doloženo) pro vytvoření. Tyto pokyny obsahují:

* Jak toobuild tento příklad hraniční sítě pomocí skriptů prostředí PowerShell.
* Jak toobuild v tomto příkladu pomocí šablony Azure Resource Manager.
* Scénáře podrobné provoz tok zobrazuje tok provozu prostřednictvím tohoto návrhu.

## <a name="references"></a>Odkazy
### <a name="helpful-websites-and-documentation"></a>Užitečné weby a dokumentace
* Přístup k Azure s Azure Resource Manager:
* Přístup k Azure pomocí prostředí PowerShell: [https://docs.microsoft.com/powershell/azureps-cmdlets-docs/](/powershell/azure/overview)
* Virtuální sítě dokumentace: [https://docs.microsoft.com/azure/virtual-network/](https://docs.microsoft.com/azure/virtual-network/)
* Dokumentace skupiny zabezpečení sítě: [https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg](virtual-network/virtual-networks-nsg.md)
* Uživatelem definované směrování dokumentace: [https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview](virtual-network/virtual-networks-udr-overview.md)
* Azure virtuální brány: [https://docs.microsoft.com/azure/vpn-gateway/](https://docs.microsoft.com/azure/vpn-gateway/)
* Site-to-Site VPN: [https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell](vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
* Dokumentace ExpressRoute (být jisti toocheck si části "Začínáme" a "Jak na" hello): [https://docs.microsoft.com/azure/expressroute/](https://docs.microsoft.com/azure/expressroute/)

<!--Image References-->
[0]: ./media/best-practices-network-security/flowchart.png "Vývojový diagram možnosti zabezpečení"
[2]: ./media/best-practices-network-security/azuresecurityfeatures.png "funkce zabezpečení azure"
[3]: ./media/best-practices-network-security/dmzcorporate.png "DMZ A v podnikové síti"
[4]: ./media/best-practices-network-security/azuresecurityarchitecture.png "Architektura zabezpečení azure"
[5]: ./media/best-practices-network-security/dmzazure.png "hraniční sítě v Azure virtuální sítě"
[6]: ./media/best-practices-network-security/dmzhybrid.png "hybridní sítě s hranicemi tři zabezpečení"
[7]: ./media/best-practices-network-security/example1design.png "Příchozí DMZ s NSG"
[8]: ./media/best-practices-network-security/example2design.png "Příchozí DMZ s hodnocení chyb zabezpečení a NSG"
[9]: ./media/best-practices-network-security/example3design.png "DMZ obousměrně s hodnocení chyb zabezpečení, NSG a UDR"
[10]: ./media/best-practices-network-security/example3firewalllogical.png "logickém zobrazení hello pravidla brány Firewall"
[11]: ./media/best-practices-network-security/example3designoptions.png "Hybridní sítě připojené DMZ s hodnocení chyb zabezpečení"
[12]: ./media/best-practices-network-security/example4designs2s.png "Hraniční sítě s hodnocení chyb zabezpečení sítě, které jsou připojené pomocí sítě Site-to-Site VPN"
[13]: ./media/best-practices-network-security/example4networklogical.png "logické sítě z hlediska hodnocení chyb zabezpečení"
[14]: ./media/best-practices-network-security/example5designoptions.png "Hraniční sítě se služba Azure Gateway propojená síť Site-to-Site hybridní"
[15]: ./media/best-practices-network-security/example5designs2s.png "Hraniční sítě s Azure bránu pomocí sítě Site-to-Site VPN"
[16]: ./media/best-practices-network-security/example6designoptions.png "Hraniční sítě se služba Azure Gateway propojená síť hybridní ExpressRoute"
[17]: ./media/best-practices-network-security/example6designexpressroute.png "Hraniční sítě s Azure bránu pomocí připojení ExpressRoute"

<!--Link References-->
[TrustCenter]: https://azure.microsoft.com/support/trust-center/compliance/
[Example1]: ./virtual-network/virtual-networks-dmz-nsg.md
[Example2]: ./virtual-network/virtual-networks-dmz-nsg-fw-asm.md
[Example3]: ./virtual-network/virtual-networks-dmz-nsg-fw-udr-asm.md
[Example4]: ./virtual-network/virtual-networks-hybrid-s2s-nva-asm.md
[Example5]: ./virtual-network/virtual-networks-hybrid-s2s-agw-asm.md
[Example6]: ./virtual-network/virtual-networks-hybrid-expressroute-asm.md
[Example7]: ./virtual-network/virtual-networks-vnet2vnet-direct-asm.md
[Example8]: ./virtual-network/virtual-networks-vnet2vnet-transit-asm.md
