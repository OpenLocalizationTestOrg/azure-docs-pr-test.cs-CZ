---
title: "Osvědčené postupy zabezpečení sítě aaaAzure | Microsoft Docs"
description: "Tento článek obsahuje sadu osvědčené postupy pro použití zabezpečení síťových součástí možnosti Azure."
services: security
documentationcenter: na
author: TomShinder
manager: swadhwa
editor: TomShinder
ms.assetid: 7f6aa45f-138f-4fde-a611-aaf7e8fe56d1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/09/2017
ms.author: TomSh
ms.openlocfilehash: 5867dea358b4da65c65b3e52fcab7e687e981642
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-network-security-best-practices"></a>Osvědčené postupy zabezpečení sítě Azure
Microsoft Azure umožňuje zařízení tooother síti virtuálních počítačů a zařízení tooconnect tím, že je na virtuálních sítí Azure. Virtuální síť Azure je konstrukce virtuální sítě, který vám umožní tooconnect virtuální sítě rozhraní karty tooa virtuální sítě tooallow založené na protokolu TCP komunikace mezi síťových zařízení. Virtuální počítače Azure tooan připojené virtuální sítě Azure jsou možné tooconnect toodevices na hello stejné virtuální síti Azure, jinou virtuální sítí Azure, na hello Internet nebo dokonce na vlastní místní sítě.

V tomto článku se budeme zabývat kolekce osvědčené postupy zabezpečení sítě Azure. Tyto doporučené postupy jsou odvozeny od našich zkušeností s prací v síti Azure a hello prostředí zákazníků, jako sami.

Pro každý osvědčený postup vysvětlíme:

* Jaké hello osvědčeným postupem je
* Proč chcete tooenable, že osvědčeným postupem
* Pokud selže tooenable hello osvědčený postup, co mohou být výsledek hello
* Osvědčeným postupem toohello možné alternativy
* Jak můžete získat informace tooenable hello osvědčený postup

Tento článek osvědčené postupy zabezpečení sítě Azure je založen na konsenzu stanovisko a možnostech platformy Azure a sady funkcí, které jsou ve hello dobu, kdy byla zapsána v tomto článku. Názory a technologie mění v čase a budou v tomto článku v pravidelných intervalech tooreflect aktualizovat tyto změny.

Azure sítě osvědčené postupy zabezpečení popsané v tomto článku patří:

* Logicky segment podsítě
* Řízení chování směrování
* Povolení vynuceného tunelování
* Pomocí virtuálních síťových zařízení
* Nasazení zóny DMZ pro rozdělení na zóny zabezpečení
* Vyhněte se ohrožení toohello Internet s vyhrazené linky WAN
* Optimalizovat výkon a provozuschopnost
* Použít globální zátěže
* Zakázat přístup RDP tooAzure virtuální počítače
* Povolit Azure Security Center
* Rozšíření vašeho datového centra do Azure

## <a name="logically-segment-subnets"></a>Logicky segment podsítě
[Virtuální sítě Azure](https://azure.microsoft.com/documentation/services/virtual-network/) jsou podobné tooa LAN ve vaší místní síti. Hello myšlenkou virtuální síť Azure je, že můžete vytvořit jeden privátní IP adresu na základě místa síti ve kterém můžete umístit všechny vaše [virtuální počítače Azure](https://azure.microsoft.com/services/virtual-machines/). Hello privátní adresní prostory IP adres k dispozici jsou hello třídy A (10.0.0.0/8), třídy B (172.16.0.0/12) a třída C rozsahy (192.168.0.0/16).

Podobně jako toowhat můžete místně, budete muset toosegment hello větší adresní prostor do podsítě. Můžete použít [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) na základě zásad toocreate zapojení podsítě.

Směrování mezi podsítěmi dojde automaticky a není nutné konfigurovat toomanually směrovacích tabulek. Ale hello výchozí nastavení je, že neexistují žádná opatření přístup sítě mezi podsítěmi hello, které vytvoříte na hello Azure Virtual Network. V pořadí toocreate sítě řízení přístupu mezi podsítěmi budete potřebovat tooput něco mezi podsítěmi hello.

Jednou z věcí hello můžete použít tooaccomplish tato úloha je [skupinu zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) (NSG). Skupiny Nsg jsou jednoduché stavové paketu kontroly zařízení, která používají hello 5-n-tice (hello zdrojové IP adresy, zdrojového portu, cílové adresy IP, cílový port a protokol vrstvy 4) přístupu toocreate povolit nebo odepřít pravidla pro provoz v síti. Můžete povolit nebo odepřít tooand provoz z jedné IP adrese, tooand z více IP adres nebo i tooand z celé podsítě.

Pomocí skupin Nsg pro řízení přístupu k síti mezi podsítěmi vám umožní tooput prostředky, které patří toohello stejné zóny zabezpečení nebo role ve svých vlastních podsítích. Například vezměte v úvahu jednoduché 3vrstvé aplikace, které má webovou vrstvu, aplikační vrstvy logiky a databázové vrstvy. Můžete uvést virtuální počítače, které patří do svých vlastních podsítích tooeach tyto vrstvy. Pak použijete skupiny Nsg toocontrol provoz mezi podsítěmi hello:

* Virtuální počítače vrstvy webové mohou iniciovat pouze připojení toohello aplikace logiky počítače a můžete akceptovat jenom připojení z Internetu hello
* Aplikace logiky virtuálních počítačů může spustit pouze připojení s databázové vrstvy a můžete akceptovat jenom připojení z hello webová vrstva
* Virtuální počítače vrstvy databáze nelze navázat spojení s nic mimo vlastní podsítě a můžete pouze přijmou připojení z vrstvy logiku aplikace hello

toolearn více o skupiny zabezpečení sítě a jak lze využít toologically segment virtuálních sítí Azure, přečtěte si článek hello [co je skupina zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) (NSG).

## <a name="control-routing-behavior"></a>Řízení chování směrování
Při umístění virtuálního počítače na virtuální síť Azure, můžete si všimnout, že hello virtuální počítač mohl připojit tooany jiným virtuálním počítačem na hello stejnou virtuální síť Azure, i když hello jiné virtuální počítače jsou v různých podsítích. Hello důvod, proč je možné je, že je kolekce tras systému, které jsou ve výchozím nastavení povolené, které povolit tento typ komunikace. Tyto výchozí trasy povolí virtuální počítače na hello stejné virtuální síti Azure tooinitiate připojení mezi sebou a se hello Internet (pro odchozí komunikace toohello Internet pouze).

Hello výchozí systémové trasy jsou užitečné pro mnoho scénářů nasazení, jsou časy, kdy chcete konfigurace směrování hello toocustomize pro vaše nasazení. Toto vlastní nastavení vám umožní tooconfigure hello další směrování adres tooreach určitým příjemcům.

Doporučujeme, abyste při nasazení virtuální síťové zařízení zabezpečení, který budeme mluvit o v novější doporučujeme konfigurovat trasy definované uživatelem.

> [!NOTE]
> Trasy definované uživatelem nejsou vyžadovány, a ve většině případů bude fungovat hello výchozí systémové trasy.
>
>

Další informace o trasy definované uživatelem a jak tooconfigure je přečíst článek hello [co jsou trasy definované uživatelem a předávání IP](../virtual-network/virtual-networks-udr-overview.md).

## <a name="enable-forced-tunneling"></a>Povolení vynuceného tunelování
toobetter pochopit vynucené tunelování, je užitečné toounderstand jaké "dělené tunelové propojení" je.
Nejběžnější příklad Hello dělené tunelové propojení se seznámili s připojeními VPN typu. Představte si vytvořit připojení VPN z vaší podnikové sítě hotelů místnosti tooyour. Toto připojení umožní vám tooconnect tooresources ve vaší podnikové síti a přejděte všechny tooresources komunikace ve vaší podnikové síti prostřednictvím tunelu VPN hello.

Co se stane, když chcete, aby tooconnect tooresources na hello Internetu? Pokud dělené tunelové propojení je povolena, tato připojení přejděte přímo toohello Internet a ne prostřednictvím hello VPN tunelu. Některé odborníky na zabezpečení, zvažte tato toobe představuje potenciální riziko a proto doporučujeme, aby dělené tunelové propojení zakázané a všechna připojení, jsou určené pro hello Internet a ty určené pro podnikové prostředky, přejděte prostřednictvím tunelu VPN hello. Výhodou Hello tato funkce je tohoto připojení toohello Internetu jsou pak vynutit prostřednictvím hello podnikové síti zabezpečovací zařízení, které nebude případ hello, pokud klient VPN hello připojený toohello Internetu mimo hello tunelového připojení sítě VPN.

Nyní Pojďme Přineste to obnovit toovirtual počítače ve virtuální síti Azure. Hello výchozí trasy pro virtuální síť Azure povolí virtuální počítače tooinitiate provoz toohello Internetu. To příliš může představovat bezpečnostní riziko, jak tyto odchozí připojení může zvýšit prostor pro útoky hello virtuálního počítače a útočníci využít.
Z tohoto důvodu doporučujeme, abyste povolili vynucené tunelování na virtuálních počítačích, když máte připojení mezi různými místy mezi vaší virtuální sítě Azure a v místní síti. Bude v souvislosti mezi místní připojení později v tomto Azure sítě osvědčené postupy dokumentu.

Pokud nemáte připojení mezi více místy, ujistěte se, můžete využít výhod skupin zabezpečení sítě (popsané výše) nebo Azure virtuální sítě zabezpečení zařízení (popsáno vedle) tooprevent odchozí připojení toohello Internetu z vaší virtuální Azure Počítače.

Další informace o toolearn vynuceného tunelování a jak tooenable, přečtěte si článek hello [nakonfigurujte vynucené tunelování prostřednictvím Powershellu a Azure Resource Manager](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md).

## <a name="use-virtual-network-appliances"></a>Pomocí virtuálních síťových zařízení
Při skupiny zabezpečení sítě a směrování definovaného uživatele může poskytnout míru zabezpečení sítě v hello sítě a transportní vrstvy hello [OSI model](https://en.wikipedia.org/wiki/OSI_model), budou toobe situace, kdy budete má nebo potřebovat tooenable zabezpečení na vysoké úrovni hello zásobníku. V takových situacích doporučujeme nasadit virtuální sítě zabezpečovací zařízení poskytovaných Azure partnery.

Síť Azure zabezpečovací zařízení doručovat výrazně rozšířené úrovně zabezpečení přes zajišťuje řízení na úrovni sítě. Mezi možnosti zabezpečení síťového hello poskytované virtuální sítě zabezpečovací zařízení patří:

* Fungující brána firewall může
* Zjišťování neoprávněných vniknutí/narušení prevence
* Ohrožení zabezpečení správy
* Řízení aplikace
* Detekce anomálií založený na síti
* Filtrování webových
* Antivirové
* Botnet ochrany

Pokud potřebujete vyšší úroveň zabezpečení sítě, než lze získat pomocí řízení přístupu na úrovni sítě, pak doporučujeme prozkoumat a nasadit virtuální síť Azure zabezpečovací zařízení.

toolearn, o jaké virtuální síť Azure zabezpečovací zařízení jsou k dispozici a jejich funkce, navštivte hello [Azure Marketplace](https://azure.microsoft.com/marketplace/) a vyhledejte "zabezpečení" a "zabezpečení sítě".

## <a name="deploy-dmzs-for-security-zoning"></a>Nasazení zóny DMZ pro rozdělení na zóny zabezpečení
DMZ "hraniční sítě" je fyzický nebo segment logické sítě, který je určený tooprovide další vrstvu zabezpečení mezi prostředky a hello Internetu. Hello záměr hello DMZ je tooplace specializuje síťových zařízení řízení přístupu v hello okraje hello DMZ sítě tak, aby pouze požadované provoz je povolený hello síťové zabezpečení zařízení a do virtuální sítě Azure.

Zóny DMZ jsou užitečné, protože sledování, protokolování a vytváření sestav v zařízeních hello na hranici hello virtuální sítě Azure se můžou zaměřit správu řízení přístupu vaší sítě. Tady by obvykle povolit DDoS prevence, zjišťování/narušení prevence vniknutí systémy (ID nebo IP adresy), pravidla brány firewall a zásady, filtrování webových, antimalwarových sítě a další. zařízení zabezpečení sítě Hello nacházejí mezi hello Internetu a virtuální sítí Azure a rozhraní na obě sítě.

I když jde o základní koncepci hello DMZ, existuje mnoho různých návrzích DMZ, například typu back-to-back tři adresami, více adresami a další.

Doporučujeme pro všechny vysoce zabezpečených nasazeních zvažte nasazení DMZ tooenhance hello úroveň zabezpečení sítě pro vaše prostředky Azure.

Další informace o zóny DMZ a jak toodeploy je v Azure, přečtěte si článek hello toolearn [cloudové služby Microsoftu a zabezpečení sítě](../best-practices-network-security.md).

## <a name="avoid-exposure-toohello-internet-with-dedicated-wan-links"></a>Vyhněte se ohrožení toohello Internet s vyhrazené linky WAN
Mnoho organizací rozhodli hello hybridního IT trasy. V hybridní IT některé společnosti hello informace prostředky jsou v Azure, zatímco ostatní zůstanou místní. V mnoha případech některé součásti služby bude používat v Azure při další součásti zůstanou místní.

V hello hybridní scénář IT je obvykle nějaký typ připojení mezi různými místy. To mezi různými místy, připojení umožňuje hello společnosti tooconnect své místní sítě tooAzure virtuální sítě. Nejsou k dispozici dvě řešení připojení mezi různými místy:

* Site-to-site VPN
* ExpressRoute

[Site-to-site VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) představuje virtuální privátní připojení mezi místní sítí a virtuální síti Azure. Proběhne tato připojení přes hello Internet a můžete příliš "tunelování" informací uvnitř šifrované odkaz mezi vaší sítí a Azure. Site-to-site VPN je zabezpečený, Vyspělá technologie, která byla nasazena podniky všech velikostí pro dekád. Tunelové propojení šifrování se provádí pomocí [režimu tunelového připojení IPsec](https://technet.microsoft.com/library/cc786385.aspx).

Při site-to-site VPN je důvěryhodné, spolehlivým a zavedených technologie, provoz v rámci hello tunel procházení hello Internetu. Kromě toho šířka pásma je poměrně omezené tooa maximálně o 200 MB/s.

Pokud budete potřebovat výjimečných úroveň zabezpečení a výkonu pro připojení mezi různými místy, doporučujeme použít Azure ExpressRoute pro připojení mezi různými místy. ExpressRoute je vyhrazené sítě WAN propojení mezi místní umístění nebo poskytovatele hostingu serveru Exchange. Protože se jedná o telco připojení, data se nebude procházet přes hello Internet a je proto není zveřejněné toohello potenciální rizika spojená s internetovou komunikaci.

Další informace o tom, jak funguje Azure ExpressRoute toolearn a jak toodeploy, přečtěte si článek hello [technický přehled ExpressRoute](../expressroute/expressroute-introduction.md).

## <a name="optimize-uptime-and-performance"></a>Optimalizovat výkon a provozuschopnost
Utajení, integrita a dostupnost (c oddílu) tvoří hello chaloupka dnešní nejvíce míry modelu zabezpečení. Důvěrnost o šifrování a ochrany osobních údajů, integritu je o tom, jak se, že data se nezmění neoprávněným uživatelem a dostupnosti je o tom, jak se, že jsou autorizované jednotlivce možné tooaccess hello informace, které mají oprávnění tooaccess. Selhání v některém z těchto oblastí představuje případný průnik v zabezpečení.

Dostupnost můžete představit jako o provozu a výkonu. Pokud služba je vypnutý, není přístupná informace. Pokud je jako data hello toomake nepoužitelný, takže nízký výkon, pak jsme můžete zvážit toobe hello dat nepřístupný. Proto z hlediska zabezpečení, je třeba toodo ať můžeme toomake našich služeb mít optimální dostupnost a výkon.
Metoda oblíbených a efektivní používá tooenhance dostupnosti a výkonu toouse Vyrovnávání zatížení. Vyrovnávání zatížení je metoda distribucí síťový provoz mezi servery, které jsou součástí služby. Například pokud máte front-end webové servery jako součást služby, můžete použít vyrovnávání zatížení toodistribute hello provoz napříč více front-endu webových serverů.

Této distribuce přenosů zvýšíte dostupnost, protože jeden z hello webových serverů přestane být dostupný, nástroj pro vyrovnávání zatížení hello zastaví odesílání přenosů toothat serveru a přesměrování provozu toohello servery, které jsou stále online. Vyrovnávání zatížení taky pomáhá výkon, protože hello procesoru, sítě a paměti režie pro obsluhovat požadavky je distribuován do všech serverů s vyrovnáváním zatížení hello.

Doporučujeme vám, že nepoužijete Vyrovnávání zatížení, kdykoli je to možné a podle potřeby pro vaše služby. Jsme budete adres vhodnost v následující části hello.
V hello úrovni virtuální sítě Azure Azure poskytuje že k tři primární možnostech Vyrovnávání zatížení:

* Vyrovnávání zatížení založené na protokolu HTTP
* Externí Vyrovnávání zatížení
* Interní vyrovnávání zatížení

## <a name="http-based-load-balancing"></a>Vyrovnávání zatížení založené na protokolu HTTP
Vyrovnávání zatížení založené na protokolu HTTP základny rozhodnutí o připojení toosend serveru, která pomocí vlastnosti protokolu hello protokolu HTTP. Azure má Vyrovnávání zatížení HTTP, který prochází hello název aplikační brány.

Doporučujeme vám nám Azure Application Gateway při:

* Aplikace, které vyžadují požadavky z hello stejné tooreach relace uživatele/client hello stejnou back-end virtuálního počítače. Příklady tohoto by nákupního košíku aplikace a webové servery e-mailu.
* Aplikace, které chcete toofree webové serverové farmy z ukončení protokolu SSL režie využitím Application Gateway [přesměrování zpracování SSL](https://f5.com/glossary/ssl-offloading) funkce.
* Aplikace, jako je například sítě pro doručování obsahu, které vyžadují více požadavky HTTP na hello stejné toobe připojení TCP dlouho běžící v směrují nebo načíst vyrovnáváním toodifferent back-end serverů.

toolearn informace o tom, jak funguje Azure Application Gateway a jak můžete použít v nasazeních, přečtěte si článek hello [Přehled brány aplikace](../application-gateway/application-gateway-introduction.md).

## <a name="external-load-balancing"></a>Externí Vyrovnávání zatížení
Vyrovnávání zatížení externí probíhá po příchozí připojení z Internetu hello mezi vaše servery umístěné v Azure Virtual Network skupinu s vyrovnáváním zatížení. Nástroje pro vyrovnávání zatížení Azure externí Hello můžete zadat, tato funkce a doporučujeme vám, že slouží jako nevyžadují hello trvalé relace nebo snižování zátěže protokolu SSL.

Na rozdíl od na základě tooHTTP Vyrovnávání zatížení, hello externí Vyrovnávání zatížení využívá informace na hello sítě a transportní vrstvy hello OSI modelu toomake rozhodnutí o sítě na jaké serveru tooload vyrovnávání připojení k.

Doporučujeme použít vyrovnávání zatížení externí vždy, když máte [bezstavové aplikace](http://whatis.techtarget.com/definition/stateless-app) přijímá příchozí žádosti od hello Internetu.

toolearn informace o tom, jak hello Azure externí nástroj pro vyrovnávání zatížení funguje a jak můžete nasadit, přečtěte si článek hello [začínáte s vytvářením k Internetu čelí pro vyrovnávání zatížení ve Správci prostředků pomocí prostředí PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="internal-load-balancing"></a>Interní Vyrovnávání zatížení
Vyrovnávání zatížení pro vnitřní je podobné tooexternal Vyrovnávání zatížení a hello používá stejný mechanismus tooload vyrovnávání připojení toohello servery za ně. Hello jenom rozdíl je, že hello Vyrovnávání zatížení v tomto případě je přijímat připojení z virtuálních počítačů, které nejsou na hello Internet. Ve většině případů hello připojení, které jsou podmínky přijaty ve Vyrovnávání zatížení zahájili zařízení v Azure Virtual Network.

Doporučujeme vám, že používáte interní Vyrovnávání zatížení pro scénáře, které budou využívat výhody tato funkce, například když potřebujete tooload vyrovnávání připojení tooSQL servery nebo interní webové servery.

toolearn informace o tom, jak funguje Azure interní Vyrovnávání zatížení a jak je můžete nasadit, přečtěte si článek hello [začínáte s vytvářením interní pro vyrovnávání zatížení pomocí prostředí PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md#update-an-existing-load-balancer).

## <a name="use-global-load-balancing"></a>Použít globální zátěže
Veřejný cloud computing je umožněno toodeploy globálně distribuované aplikace, které mají součásti, které jsou umístěné v datových centrech po celém hello, world. To je možné v Microsoft Azure z důvodu přítomnosti tooAzure na globální datacenter. Na rozdíl od technologie Vyrovnávání zatížení toohello již bylo zmíněno dříve, Vyrovnávání zatížení globální je možné toomake služby k dispozici i v případě, že celá datová centra může být k dispozici.

Tento typ globální Vyrovnávání zatížení v Azure a využívají můžete získat [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/). Díky Traffic Manager je možné tooload režimu připojení tooyour služeb v závislosti na umístění hello hello uživatele.

Například hello uživatel upravuje služby tooyour žádosti z hello Evropa, hello připojení je řízené tooyour služby umístěné v datacentrum Evropa. Tato část pomůže tooimprove výkonu globální zátěže Traffic Manager vzhledem k tomu, že připojení toohello nejbližší datacenter je rychlejší než připojení toodatacenters, které jsou daleko.

Na straně dostupnosti hello Vyrovnávání zatížení globální zajišťuje, že vaše služba je k dispozici i v případě, že by měl k dispozici celého datového centra.

Například, pokud datovém centru Azure by měl k dispozici z důvodu tooenvironmental důvodů nebo termín toooutages (například selhání k místní síti), připojení bude tooyour služby přesměruje toohello nejbližší online datacenter. Tato služba Vyrovnávání zatížení globální dosahuje využitím DNS zásady, které můžete vytvořit v Traffic Manageru.

Doporučujeme používat pro řešení cloudu, které vyvíjíte a který má obor široce distribuované nad několika oblastmi a vyžaduje hello nejvyšší úroveň provozu možné Traffic Manager.

více o Azure Traffic Manager toolearn a jak toodeploy, přečtěte si článek hello [co je Traffic Manager](../traffic-manager/traffic-manager-overview.md).

## <a name="disable-rdpssh-access-tooazure-virtual-machines"></a>Zakázat přístup RDP/SSH tooAzure virtuální počítače
Je možné tooreach virtuální počítače Azure pomocí hello [Remote Desktop Protocol](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) (RDP) a hello [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) protokoly (SSH). Tyto protokoly, takže bude možné toomanage virtuálních počítačů ze vzdálených umístění a jsou v datovém centru computing standardní.

Hello potenciální problém zabezpečení s použitím těchto protokolů přes hello Internet se útočníci mohou používat různé [hrubou silou](https://en.wikipedia.org/wiki/Brute-force_attack) techniky toogain přístup tooAzure virtuálních počítačů. Jakmile hello útočníci získají přístup, mohou použít virtuální počítač jako bod spuštění pro ostatní počítače v Azure Virtual Network narušení nebo i útokům síťová zařízení mimo Azure.

Z toho důvodu doporučujeme zakázat přímé RDP a SSH přístup tooyour virtuálních počítačů Azure z hello Internetu. Po direct RDP a SSH přístup z hello zakázali Internetu, máte další možnosti, můžete použít tooaccess těchto virtuálních počítačů pro vzdálenou správu:

* Point-to-site VPN
* Site-to-site VPN
* ExpressRoute

[Point-to-site VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) je jiný termín pro připojení klienta nebo serveru sítě VPN pro vzdálený přístup. Síť VPN point-to-site umožňuje tooan tooconnect jednoho uživatele Azure Virtual Network prostřednictvím hello Internetu. Po připojení point-to-site hello, hello uživatel nebude moct toouse protokolu RDP nebo SSH tooconnect tooany virtuální počítače nachází na hello Azure Virtual Network které hello uživatele připojené toovia point-to-site VPN. Předpokladem je, že uživatele hello je autorizovaný tooreach tyto virtuální počítače.

Point-to-site VPN je bezpečnější než přímé připojení protokolu RDP nebo SSH, protože má uživatel hello tooauthenticate dvakrát před připojování tooa virtuálního počítače. Nejprve hello tooauthenticate potřebám uživatele (a autorizaci) připojení VPN point-to-site tooestablish hello; druhý, hello tooauthenticate potřebám uživatele (a autorizaci) tooestablish hello protokolu RDP nebo SSH relace.

A [site-to-site VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) se připojí k síti tooanother celá síť prostřednictvím hello Internetu. Můžete vytvořit site-to-site VPN tooconnect tooan vaší místní síti Azure Virtual Network. Pokud nasadíte site-to-site VPN, uživatelé ve vaší místní síti bude možné tooconnect toovirtual počítačů ve vaší virtuální síti Azure pomocí hello protokolu RDP nebo protokol SSH přes hello připojení site-to-site VPN a nevyžaduje tooallow přímé protokolu RDP nebo SSH přístup přes hello Internet.

Můžete taky podobné toohello vyhrazené propojení WAN tooprovide funkce site-to-site VPN. Hlavní rozdíly Hello se 1. vyhrazené propojení WAN Hello není procházení hello Internet a 2. vyhrazené linky WAN jsou obvykle další stabilní a původce. Azure poskytuje řešení vyhrazené propojení WAN ve formě hello [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/).

## <a name="enable-azure-security-center"></a>Povolit Azure Security Center
Azure Security Center pomáhá zabránit, zjistit a reagovat toothreats a poskytuje že vyšší přehled a kontrolu nad, hello zabezpečení vašich prostředků Azure. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných Azure, pomáhá zjišťovat hrozby, kterých byste si jinak nevšimli, a spolupracuje s řadou řešení zabezpečení.

Azure Security Center pomáhá optimalizovat a monitorování zabezpečení sítě podle:

* Poskytuje doporučení zabezpečení sítě
* Monitorování stavu hello konfigurace zabezpečení sítě
* Výstrahy upozorňující na základě toonetwork hrozeb obou úrovni hello koncový bod a sítě

Důrazně doporučujeme, abyste povolili pro všechna vaše nasazení Azure Azure Security Center.

Další informace o službě Azure Security Center a jak tooenable pro vaše nasazení, přečtěte si článek hello toolearn [Úvod tooAzure Security Center](../security-center/security-center-intro.md).

## <a name="securely-extend-your-datacenter-into-azure"></a>Bezpečně rozšířit vašeho datového centra do Azure
Mnoho podnikových IT organizace hledáte tooexpand do cloudu hello místo narůstají jejich místní datacentra. Toto rozšíření představuje rozšíření stávající infrastruktury IT do veřejného cloudu hello. Využitím mezi různými místy je možné tootreat možnosti připojení virtuálních sítí Azure jako jakékoli jiné podsíti na místní síťové infrastruktuře.

Ale existuje mnoho plánování a návrhu problémy, které je třeba toobe řešeny první. To je obzvláště důležité v oblasti hello zabezpečení sítě. Jeden hello doporučené způsoby toounderstand přístup takový návrh je toosee příklad.

Společnost Microsoft vytvořila hello [Diagram architektury odkaz rozšíření Datacenter](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84#content) a podpora vedlejší toohelp chápete, co bude vypadat takové příponu datového centra. To poskytuje příklad implementace odkaz, můžete použít tooplan a návrhu cloudu toohello zabezpečené enterprise datacenter rozšíření. Doporučujeme, abyste si prošli tento dokument tooget představu o hello klíčové komponenty zabezpečené řešení.

toolearn Další informace o tom, jak rozšířit toosecurely vašeho datového centra do Azure, zobrazte hello video [tooMicrosoft rozšíření vašeho datového centra Azure](https://www.youtube.com/watch?v=Th1oQQCb2KA).
