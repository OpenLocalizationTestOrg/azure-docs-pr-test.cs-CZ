---
title: "aaaAzure zabezpečení sítě | Microsoft Docs"
description: "Další informace o cloudové výpočetní služby, které zahrnují široký výběr výpočetních instancích & služby, které je možné škálovat nahoru a dolů automaticky toomeet hello potřebám vaší aplikace nebo enterprise."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/24/2017
ms.author: TomSh
ms.openlocfilehash: 7dae83bbe338a2727575447583a7fb773527dd59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-network-security"></a>Zabezpečení sítě Azure

Víme, že je zabezpečení úlohy, jeden v cloudu hello a jak důležité je, že zjistíte přesné a aktuální informace o zabezpečení Azure. Jedním z hello nejlepší toouse důvodů Azure pro vaše aplikace a služby je tootake výhod Azure širokou škálu zabezpečení nástroje a možnosti. Tyto nástroje a možnosti pomáhat při jeho možné toocreate zabezpečené řešení na hello platformy Azure.

Microsoft Azure poskytuje utajení, integrita a dostupnost dat zákazníka, a zároveň umožnit transparentní odpovědnosti za. toohelp lépe pochopit hello kolekce ovládacích prvků zabezpečení sítě, která je implementovaná v rámci Microsoft Azure z hlediska hello zákazníka, tento článek "Azure Network Security", je zapsán tooprovide komplexní pohled na hello zabezpečení sítě ovládací prvky s Microsoft Azure k dispozici.

Tento dokument je určený, tooinform ohledně hello širokou škálu síťové ovládací prvky, které můžete konfigurovat zabezpečení hello tooenhance hello řešení, který můžete nasadit v Azure. Pokud vás zajímá v jak společnost Microsoft infrastruktura sítě hello toosecure hello samotné platformě Azure, najdete v části zabezpečení Azure hello v hello [Microsoft Trust Center](https://www.microsoft.com/trustcenter/security/azure-security).

## <a name="azure-platform"></a>Platforma Azure

Azure je platforma služby veřejného cloudu, která podporuje široký výběr operačních systémů, programovací jazyky, rozhraní, nástroje, databází a zařízení.  Může probíhat Linux kontejnery s integrace Dockeru; vývoj aplikací pomocí jazyka JavaScript, Python, .NET, PHP, Java a Node.js; sestavení back EndY pro iOS, Android a Windows zařízení. Cloudové služby Azure podporují hello stejné technologie miliony vývojářů a IT profesionály už spoléhají na a vztah důvěryhodnosti.

Když sestavení, nebo migrovat prostředky IT, poskytovatele služeb veřejného cloudu, se spoléhat na dané organizace dalo tooprotect aplikace a data s hello služeb a hello ovládací prvky poskytují toomanage hello zabezpečení vašeho cloudu prostředky.

Infrastruktury Azure a je určená z hello tooapplications zařízení pro hostování miliony zákazníků současně, a poskytuje trustworthy foundation, na kterém může podnikům splňovat požadavky jejich zabezpečení. Kromě toho Azure poskytuje rozsáhlou sadu toocontrol možnost konfigurovat zabezpečení možnosti a hello je tak, aby si můžete přizpůsobit toomeet hello jedinečné požadavky na zabezpečení vaší organizace nasazení.

## <a name="abstract"></a>Abstraktní

Veřejné cloudové služby společnosti Microsoft poskytování služeb flexibilně škálovatelné a infrastruktury, podnikové úrovni funkce a mnoho možností pro hybridní připojení. Tooaccess můžete tyto služby prostřednictvím hello Internetu nebo s Azure ExpressRoute, které poskytuje připojení k privátní síti. Platforma Microsoft Azure Hello vám umožní tooseamlessly rozšířit vaši infrastrukturu do cloudu hello a vytvářet vícevrstvé architektury. Kromě toho můžete povolit třetím stranám rozšířené možnosti prostřednictvím nabídky zabezpečení služeb a virtuálních zařízení.

Síťové služby Azure maximalizovat flexibilitu, dostupnosti, odolnost proti chybám, zabezpečení a integrity záměrné. Tento dokument poskytuje informace o hello síťové funkce Azure a informace o tom, jak zákazníci mohou používat Azure nativním bezpečnostním funkce toohelp ochranu svých prostředků informací.

Hello zamýšlená cílové skupiny pro tento dokument White Paper patří:

- Technické správci, správci sítě a vývojáři, kteří hledají řešení zabezpečení dostupným a podporovaným v Azure.

-   Malé a střední podniky nebo obchodní proces vedení, kteří chtějí tooget přehled hello Azure síťové technologie a služby, které jsou relevantní diskuze kolem zabezpečení sítě v hello veřejného cloudu Azure.

## <a name="azure-networking-big-picture"></a>Velký obrázek sítě Azure
Microsoft Azure obsahuje robustní sítě toosupport infrastruktury, aplikace a požadavky na připojení služby. Připojení k síti je možné mezi prostředky, které jsou umístěné v Azure, mezi místními a Azure hostované prostředky a tooand z hello Internet a Azure.

![Velký obrázek sítě Azure](media/azure-network-security/azure-network-security-fig-1.png)

Hello [Azure síťové infrastruktury](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-networking-guidelines) umožňuje toosecurely můžete připojit prostředky Azure tooeach jiné s virtuálními sítěmi (virtuální sítě). Virtuální síť je reprezentace vlastní sítě v cloudu hello. Virtuální síť je to logická izolace hello cloudu Azure síť vyhrazený tooyour předplatného. Můžete propojit virtuální sítě tooyour místní sítě.

Azure podporuje vyhrazený propojení WAN připojení tooyour do místní sítě a virtuální síť Azure s [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction). Hello propojení mezi Azure a váš web používá vyhrazené připojení, které nejde přes hello veřejného Internetu. Pokud vaše aplikace Azure běží v několik datových center, můžete použít [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) tooroute žádosti od uživatelů inteligentně mezi instancemi aplikace hello. Také je možné směrovat provoz tooservices není spuštěna v Azure, pokud jsou přístupné z Internetu hello.

## <a name="enterprise-view-of-azure-networking-components"></a>Zobrazení Enterprise Azure síťovými součástmi
Azure má mnoho síťové součásti, které jsou relevantní toonetwork diskusí zabezpečení. jsme popisují tyto síťovými součástmi a zaměřit se na zabezpečení hello problémy související toothem.

> [!Note]
> Ne všechny aspekty Azure sítě jsou popsány – probereme pouze ty, které jsou za toobe hrají v plánování a navrhování infrastruktury zabezpečené sítě kolem služby a aplikace, které můžete nasadit v Azure.

V tomto dokumentu bude se titulní hello následující sítě s podnikovými Azure:

-   Základní připojení k síti

-   Hybridní připojení

-   Ovládací prvky zabezpečení

-   Ověření sítě

### <a name="basic-network-connectivity"></a>Základní připojení k síti

Hello [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) služby umožňuje toosecurely můžete připojit prostředky Azure tooeach jiné s virtuálními sítěmi (VNet). Virtuální síť je reprezentace vlastní sítě v cloudu hello. Virtuální síť je to logická izolace hello síť Azure infrastruktury vyhrazené tooyour předplatného. Můžete se také připojit virtuální sítě tooeach jiných a místní sítí pomocí sítě site-to-site VPN a vyhrazené tooyour [linky WAN](https://docs.microsoft.com/azure/expressroute/expressroute-introduction).

![Základní připojení k síti](media/azure-network-security/azure-network-security-fig-2.png)

S hello vysvětlení, že používáte servery toohost virtuální počítače v Azure otázka hello se, jak tyto virtuální počítače připojit tooa síť. Hello odpovědí je, že virtuální počítače připojit tooan [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).

Virtuální sítě Azure jsou jako hello virtuální sítě můžete použít místní s vlastní řešení virtualizace platformy, jako je například Microsoft Hyper-V nebo VMware.

#### <a name="intra-vnet-connectivity"></a>Připojení Intra-VNet

Virtuální sítě tooeach další se můžete připojit, povolení prostředky toocommunicate tooeither virtuální síť připojení mezi sebou mezi virtuálními sítěmi. Můžete použít jednu nebo obě následující možnosti tooconnect virtuálních sítí tooeach jiných hello:

- **Partnerský vztah:** umožňuje prostředky připojené toodifferent sítě Azure Vnet v rámci hello stejné umístění Azure toocommunicate mezi sebou. Hello šířky pásma a latence napříč hello VNet je hello stejné jako v případě, že hello prostředky byly připojené toohello stejnou virtuální síť. číst informace o vytvoření partnerského vztahu, toolearn [partnerský vztah virtuální sítě](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).

 ![Partnerské vztahy](media/azure-network-security/azure-network-security-fig-3.png)

- **Připojení VNet-to-VNet:** umožňuje prostředky připojené toodifferent virtuální síť Azure v rámci hello stejný nebo jiný Azure umístění. Na rozdíl od partnerského vztahu, šířka pásma je omezená mezi virtuálními sítěmi, protože provoz musí procházet skrz bránu VPN Azure.

![Připojení VNet-to-VNet](media/azure-network-security/azure-network-security-fig-4.png)


Další informace o propojení virtuálních sítí s připojení VNet-to-VNet, přečtěte si hello toolearn [konfigurace článku připojení VNet-to-VNet](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal?toc=%2fazure%2fvirtual-network%2ftoc.json).

#### <a name="azure-virtual-network-capabilities"></a>Virtuální síť Azure možnosti:

Jak vidíte, virtuální síť Azure poskytuje virtuální počítače tooconnect toohello sítě, aby se můžete připojit tooother síťovým prostředkům zabezpečené způsobem. Základní připojení se ale jenom hello začátku. Hello následující funkce služby Azure Virtual Network hello vystavit vlastnosti zabezpečení hello virtuální sítě Azure:

-   Izolace

-   Připojení k internetu

-   Připojení prostředků Azure.

-   Připojení virtuální sítě

-   Místní připojení

-   Filtrování přenosu

-   Směrování

**Izolace**

Virtuální sítě jsou [izolované](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) od sebe navzájem. Můžete vytvořit samostatné virtuální sítě pro vývoj, testování a produkci, použijte hello stejné [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) bloky adres. Naopak můžete vytvořit více virtuálních sítí, použít jiné bloky adres CIDR a společně připojení sítě. Virtuální síť můžete rozdělit do několika podsítí.

Azure poskytuje interní překlad adres pro virtuální počítače a [cloudové služby](https://azure.microsoft.com/services/cloud-services/) instance rolí připojené tooa virtuální sítě. Volitelně můžete nakonfigurovat virtuální síť toouse vlastní servery DNS, místo použití Azure interní překlad adres.

Můžete implementovat více virtuálních sítí v rámci každé Azure [předplatné](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology?toc=%2fazure%2fvirtual-network%2ftoc.json) a Azure [oblast](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology?toc=%2fazure%2fvirtual-network%2ftoc.json). Každý virtuální sítě je izolovaná od jiných virtuálních sítí. Pro každý virtuální síť můžete:

-   Zadejte vlastní prostor privátní IP adresy pomocí veřejné a privátní adresy (RFC 1918). Azure přiřadí připojené toohello prostředky virtuální sítě privátní IP adresu z hello adresního prostoru, můžete přiřadit.

-   Segment hello virtuální sítě do jedné nebo více podsítí a přidělovat část podsíť tooeach hello virtuální síť adres místa.

-   Můžete použít Azure překlad nebo zadejte že vlastní server DNS za účelem použití prostředků připojený tooa virtuální sítě. Další informace o překlad názvů ve virtuálních sítí, přečtěte si hello toolearn [překlad názvů pro virtuální počítače a cloudové služby](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances).

**Připojení k Internetu**

Všechny [Azure virtuální počítače (VM)](https://docs.microsoft.com/azure/virtual-machines/windows/) a instance rolí cloudové služby tooa připojené virtuální sítě mají přístup k toohello Internetu, ve výchozím nastavení. Můžete také povolit příchozí přístup k prostředkům toospecific, podle potřeby. (VM) a instancí rolí cloudové služby tooa připojené virtuální sítě mají přístup k toohello Internetu, ve výchozím nastavení. Můžete také povolit příchozí přístup k prostředkům toospecific, podle potřeby.

Všechny prostředky připojené tooa virtuální síť mít toohello odchozí připojení k Internetu ve výchozím nastavení. Hello privátní IP adresu prostředku hello je, že zdrojová síťová adresa přeložen hello infrastrukturu Azure (překládat pomocí SNAT) tooa veřejnou IP adresu. Implementací vlastní směrování a filtrování provozu, můžete změnit výchozí připojení hello. Další informace o odchozí připojení k Internetu, přečtěte si hello toolearn [pochopení odchozí připojení v Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections?toc=%2fazure%2fvirtual-network%2ftoc.json).

toocommunicate příchozí tooAzure prostředky z hello Internet nebo toocommunicate odchozí toohello, které Internetu bez překládat pomocí SNAT, prostředek musí být přiřazen veřejnou IP adresu. Další informace o veřejné IP adresy, přečtěte si hello toolearn [veřejné IP adresy](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address).

**Připojení prostředků Azure.**

[Prostředky Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) , jako je cloudové služby a virtuální počítače můžou být připojené toohello stejnou virtuální síť. Hello prostředky můžete připojit tooeach jiných použití privátních IP adres, i když jsou v různých podsítích. Azure poskytuje výchozí směrování mezi podsítěmi, virtuálními sítěmi a místními sítěmi, takže nemáte tooconfigure a spravovat trasy.

Několik prostředků Azure tooa virtuální sítě, jako jsou virtuální počítače (VM), cloudové služby, prostředí App Service a sady škálování virtuálního počítače se můžete připojit. Virtuální počítače připojení tooa podsítí v rámci virtuální sítě přes síťové rozhraní (NIC). Další informace o síťových adaptérů, přečtěte si hello toolearn [síťových rozhraní](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface).

**Připojení virtuální sítě**

[Virtuální sítě](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) může být připojené tooeach jiné, povolení prostředky připojené virtuální sítě toocommunicate tooany s jakýmikoli prostředky na ostatní virtuální sítě.

Virtuální sítě tooeach další se můžete připojit, povolení prostředky toocommunicate tooeither virtuální síť připojení mezi sebou mezi virtuálními sítěmi. Můžete použít jednu nebo obě následující možnosti tooconnect virtuálních sítí tooeach jiných hello:

- **Partnerský vztah:** umožňuje prostředky připojené toodifferent sítě Azure Vnet v rámci hello stejné umístění Azure toocommunicate mezi sebou. Hello šířky pásma a latence napříč hello virtuální sítě je hello stejné jako kdyby hello prostředky připojené toohello stejné VNet.toolearn Další informace o vytvoření partnerského vztahu, přečtěte si hello [partnerský vztah virtuální sítě](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).

- **Připojení VNet-to-VNet:** umožňuje prostředky připojené toodifferent virtuální síť Azure v rámci hello stejný nebo jiný Azure umístění. Na rozdíl od partnerského vztahu, šířka pásma je omezená mezi virtuálními sítěmi, protože provoz musí procházet skrz bránu VPN Azure. Další informace o propojení virtuálních sítí s připojení VNet-to-VNet toolearn. toolearn víc, přečtěte si hello [konfigurace připojení typu VNet-to-VNet](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal?toc=%2fazure%2fvirtual-network%2ftoc.json) .

**Místní připojení**

Virtuální sítě může být připojen příliš[místní](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) sítě prostřednictvím privátní sítě připojení mezi vaší sítí a Azure, nebo připojení site-to-site VPN přes hello Internet.

Můžete připojit vaše místní síť tooa sítě VNet pomocí libovolné kombinace hello následující možnosti:

- **Point-to-site virtuální privátní sítě (VPN):** mezi jeden počítač připojený tooyour sítě a hello virtuální sítě. Tento typ připojení je skvělé, pokud jste se právě Začínáme s Azure, nebo pro vývojáře, protože vyžaduje žádné nebo téměř žádné změny tooyour stávající síť. připojení Hello používá hello SSTP protokol tooprovide zašifrovaná komunikace přes Internet hello mezi hello počítači a hello virtuální sítě. Hello latence pro síť VPN point-to-site nepředvídatelným, protože hello provoz prochází hello Internetu.

- **Site-to-site VPN:** mezi zařízení VPN a služby Azure VPN Gateway. Tento typ připojení umožňuje jakémukoli prostředku místní autorizujete tooaccess virtuální sítě. je Hello připojení VPN pomocí protokolu IPsec/IKE, která poskytuje šifrovanou komunikaci přes Internet hello mezi místní zařízení a hello Azure VPN gateway. vzhledem k tomu, že hello provoz prochází hello Internet nepředvídatelným Hello latence pro připojení site-to-site.

- **Azure ExpressRoute:** navázat mezi vaší sítí a Azure prostřednictvím partnerem ExpressRoute. Toto připojení je soukromé. Provoz neprochází hello Internetu. vzhledem k tomu, že provoz není procházení hello Internet je předvídatelný Hello latence pro připojení typu ExpressRoute. Další informace o všech hello předchozí možnosti připojení, přečtěte si hello toolearn [diagramy topologie připojení](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json).

**Filtrování přenosu**

Instance role virtuálního počítače a cloudové služby [přenos v síti](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) je možné filtrovat příchozí a odchozí zdrojové IP adresy a portu, cílové IP adresy a portu a protokolu.

Můžete filtrovat síťový provoz mezi podsítěmi pomocí jedné nebo obou hello následující možnosti:

- **Skupin zabezpečení (NSG) sítě:** každou NSG může obsahovat více zabezpečení příchozí a odchozí pravidla, která umožňují toofilter provoz podle zdrojové a cílové IP adresy, portu a protokolu. Můžete použít NSG tooeach síťový adaptér ve virtuálním počítači. Můžete taky použít podsíť toohello NSG síťový adaptér nebo jiných prostředků Azure, je připojen k. Další informace o skupin Nsg, přečtěte si hello toolearn [skupin zabezpečení sítě](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).

- **Virtuální síťová zařízení:** zařízení virtuální sítě je virtuální počítač spuštěný software, který provádí síťové funkce, jako je například Brána firewall. Zobrazte seznam dostupných NVAs v hello Azure Marketplace. NVAs jsou také k dispozici, které poskytují optimalizace sítě WAN a jiných síťových přenosů funkce. NVAs jsou obvykle používány s uživatelem nebo trasy protokolu BGP. Můžete také použít hodnocení chyb zabezpečení toofilter provoz mezi virtuálními sítěmi.

**Směrování**

Volitelně můžete přepsat výchozí Azure směrování konfiguraci vlastních tras, nebo pomocí trasy protokolu BGP prostřednictvím brány sítě.

Azure vytvoří směrovací tabulky, které umožňují prostředkům připojených tooany podsíť v toocommunicate žádné virtuální sítě s sebou ve výchozím nastavení. Můžete implementovat buď nebo obě následující možnosti toooverride hello hello výchozích tras, které vytvoří Azure:

- **Trasy definované uživatelem:** můžete vytvořit vlastní směrovací tabulky s trasami, které řídí, kde je provoz směrovaný toofor každou podsíť. Další informace o trasy definované uživatelem, přečtěte si hello toolearn [trasy definované uživatelem](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview).

- **Trasy protokolu BGP:** Pokud připojíte vaší virtuální sítě tooyour místní sítí pomocí připojení k Azure VPN Gateway nebo ExpressRoute, můžete rozšířit tooyour trasy protokolu BGP virtuální sítě.

### <a name="hybrid-internet-connectivity-connect-tooan-on-premises-network"></a>Hybridní připojení k Internetu: připojení tooan do místní sítě
Můžete připojit vaše místní síť tooa sítě VNet pomocí libovolné kombinace hello následující možnosti:

-   Připojení k internetu

-   Síť VPN Point-to-site (P2S VPN)

-   VPN typu Site-to-Site (S2S VPN)

-   ExpressRoute

#### <a name="internet-connectivity"></a>Připojení k Internetu

Jak naznačuje název připojení k Internetu, zpřístupní úlohy z hello Internet, tak, že jste vystavit tooworkloads různé veřejných koncových bodů, které za provozu ve virtuální síti hello. Tyto úlohy mohou být vystaveny pomocí [nástroj pro vyrovnávání zatížení internetového](https://docs.microsoft.com/azure/load-balancer/load-balancer-internet-overview) nebo jednoduše přiřazení veřejnou IP adresou toohello virtuálních počítačů. Tímto způsobem bude možné pro cokoli na hello Internet toobe možné tooreach tomto virtuálním počítači, poskytována bránu firewall hostitele, [skupin zabezpečení (NSG) sítě](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg), a [trasy definované uživatelem](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) povolit, toohappen.

V tomto scénáři může zveřejnit aplikaci, která potřebuje toobe veřejné toohello Internet a být schopný tooconnect tooit z kdekoli nebo z určitých umístění v závislosti na konfiguraci hello vašich zatížení.

#### <a name="point-to-site-vpn-or-site-to-site-vpn"></a>Point-to-Site VPN nebo VPN typu Site-to-Site
Tyto dvě spadá do hello stejné kategorii. Obě potřebují toohave vaší virtuální síti Brána sítě VPN a můžete připojit pomocí klienta VPN pro pracovní stanice v rámci hello tooit [KonfiguracePoint-to-Site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) nebo můžete konfigurovat místní [zařízení VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices) toobe možné tooterminate site-to-site VPN. Tímto způsobem tooresources v rámci hello virtuální sítě můžete připojit místní zařízení.

Konfigurace Point-to-Site (P2S) umožňuje vytvoření bezpečného připojení z virtuální sítě jednotlivých klientských počítačů tooa. P2S je připojení VPN prostřednictvím protokolu SSTP (Secure Socket Tunneling Protocol).

![Point-to-Site VPN](media/azure-network-security/azure-network-security-fig-5.png)

Připojení point-to-Site jsou užitečné, pokud chcete tooconnect tooyour virtuální síti ze vzdáleného umístění, například z domova nebo centrum konference, nebo pokud máte pouze několik klientů, kteří potřebují tooconnect tooa virtuální sítě.

Připojení typu P2S nevyžadují zařízení VPN ani veřejnou IP adresu. Můžete vytvořit připojení VPN hello z hello klientského počítače. Proto P2S nedoporučuje tooAzure tooconnect způsob, jak v případě, že potřebujete trvalé připojení z mnoha místní zařízení a počítačů tooyour síť Azure.

![Site-to-Site VPN](media/azure-network-security/azure-network-security-fig-6.png)

> [!Note]
> Další informace o připojení Point-to-Site najdete v tématu hello [Point-to-Site DM v Q](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal).

Připojení k bráně VPN Site-to-Site je použité tooconnect místní sítě tooan virtuální síť Azure přes tunelové propojení VPN protokolu IPsec/IKE (IKEv1 nebo IKEv2).

Tento typ připojení vyžaduje sítě VPN zařízení nachází v místě které má zvenčí veřejnou IP adresu přiřazenou tooit. Proběhne tato připojení přes hello Internet a můžete příliš "tunelování" informací uvnitř šifrované odkaz mezi vaší sítí a Azure. Site-to-site VPN je zabezpečený, Vyspělá technologie, která byla nasazena podniky všech velikostí pro dekád. Tunelové propojení šifrování se provádí pomocí [režimu tunelového připojení IPsec](https://technet.microsoft.com/library/cc786385.aspx).

Při site-to-site VPN je důvěryhodné, spolehlivým a zavedených technologie, provoz v rámci hello tunel procházení hello Internetu. Kromě toho šířka pásma je poměrně omezené tooa maximálně přibližně 200 MB/s.

Pokud budete potřebovat výjimečných úroveň zabezpečení a výkonu pro připojení mezi různými místy, doporučujeme použít Azure ExpressRoute pro připojení mezi různými místy. ExpressRoute je vyhrazené sítě WAN propojení mezi místní umístění nebo poskytovatele hostingu serveru Exchange. Protože se jedná o telco připojení, data se nebude procházet přes hello Internet a je proto není zveřejněné toohello potenciální rizika spojená s internetovou komunikaci.

> [!Note]
> Další informace o branách VPN najdete v tématu [brány VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).

#### <a name="dedicated-wan-link"></a>Vyhrazené propojení WAN
Microsoft Azure ExpressRoute umožňuje rozšířit vaše místní sítě do hello Azure přes vyhrazené soukromé připojení zajišťované poskytovatelem připojení.

Připojení ExpressRoute se nepřenášejí prostřednictvím hello veřejného Internetu. To umožňuje toooffer připojení ExpressRoute Další spolehlivost, vyšší rychlost, nižší latenci a vyšší zabezpečení než Typická připojení přes hello Internet.

![ Vyhrazené propojení WAN](media/azure-network-security/azure-network-security-fig-7.png)

> [!Note]
> Informace o tom tooconnect tooMicrosoft vaší sítě pomocí služby ExpressRoute, najdete v části [modelů připojení ExpressRoute](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) a [technický přehled ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction).

Jako s hello site-to-site VPN možnosti ExpressRoute vám také umožní tooresources tooconnect, které nejsou nezbytně jenom jeden virtuální sítě. V závislosti na hello SKU, ve skutečnosti můžete připojit too10 virtuální sítě. Pokud máte hello [doplněk premium](https://docs.microsoft.com/azure/expressroute/expressroute-faqs), tooup připojení virtuální sítě too100 je možné, v závislosti na šířku pásma. Další informace o co tyto typy připojení vzhled líbí, přečtěte si toolearn [diagramy topologie připojení](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="security-controls"></a>Ovládací prvky zabezpečení
Virtuální síť Azure poskytuje zabezpečené, logickou síť, která je izolovaná od jiných virtuálních sítí a podporuje mnoho ovládací prvky zabezpečení, která se používá ve vaší místní sítě. Zákazníci vytvořit své vlastní struktura pomocí: podsítě – budou používat svoje vlastní rozsah privátních IP adres, konfigurace směrovacích tabulek a skupin zabezpečení sítě řízení přístupu seznamy (ACL), bran a virtuálních zařízení toorun jejich úlohy v cloudu hello.

Hello následují kontrolní mechanismy zabezpečení, které můžete použít na virtuálních sítí Azure:

-   Ovládací prvky pro přístup k síti

-   Trasy definované uživatelem

-   Síťové zařízení zabezpečení

-   Application Gateway

-   Brány Firewall Azure webových aplikací

-   Řízení dostupnost sítě

#### <a name="network-access-controls"></a>Ovládací prvky pro přístup k síti
Při hello Azure Virtual Network (VNet) je hello kamenem Azure model pro sítě a poskytuje izolace a ochrany, hello [skupiny zabezpečení sítě (NSG)](https://blogs.msdn.microsoft.com/igorpag/2016/05/14/azure-network-security-groups-nsg-best-practices-and-lessons-learned/) jsou hello hlavní nástroj používáte tooenforce a řízení provozu v síti pravidla na úrovni sítě hello.

![ Ovládací prvky pro přístup k síti](media/azure-network-security/azure-network-security-fig-8.png)


Můžete řídit přístup k povolení nebo odepření komunikace mezi hello zatížení v rámci virtuální sítě, ze systémů v sítích zákazníka prostřednictvím připojení mezi různými místy, nebo přímé komunikaci v síti Internet.

V diagramu hello virtuální sítě a skupin Nsg nacházet v určité vrstvy v zásobníku hello Azure celkové zabezpečení, kde skupiny Nsg, UDR a síťových virtuálních zařízení můžou být použité toocreate zabezpečení hranice tooprotect hello aplikace nasazení v síti hello chráněný.

Skupiny Nsg použijte přenosem tooevaluate 5-n-tice (a používají hello pravidla můžete konfigurovat pro hello NSG):

-   [Zdrojové a cílové IP adresy](https://support.microsoft.com/help/969029/the-functionality-for-source-ip-address-selection-in-windows-server-2008-and-in-windows-vista-differs-from-the-corresponding-functionality-in-earlier-versions-of-windows)

-   [Zdrojový a cílový port](https://technet.microsoft.com/library/dd197515)

-   Protokol: [protokol Transmission Control Protocol (TCP)](https://technet.microsoft.com/library/cc940037.aspx) nebo [User Datagram Protocol (UDP)](https://technet.microsoft.com/library/cc940034.aspx)

To znamená, že můžete řídit přístup mezi jeden virtuální počítač a skupinu virtuálních počítačů nebo jeden virtuální počítač tooanother jeden virtuální počítač, nebo mezi celý podsítě. Znovu mějte na paměti, že toto je jednoduchá stavových paketů filtrování, kontroly paketů není úplná. Neexistuje žádné ověřovací protokol nebo ID úrovně sítě nebo IP adresy funkci skupinu zabezpečení sítě.

Skupina NSG obsahuje některá vestavěné pravidla, které byste měli vědět. Jsou to:

-   **Povolují veškerý provoz v rámci konkrétní virtuální sítě:** všechny virtuální počítače na hello stejné virtuální síti Azure můžete komunikaci mezi sebou.

-   **Povolit tooinbound Vyrovnávání zatížení Azure:** toto pravidlo umožňuje přenosy z jakékoli zdrojové adresy tooany cílové adresy pro vyrovnávání zatížení Azure hello.

-   **Odepřít všechny příchozí:** toto pravidlo blokuje veškerý provoz sourcing z Internetu, které jste povolili explicitně hello.

-   **Povolit všechny přenosy odchozí toohello Internet:** toto pravidlo umožňuje virtuálním počítačům tooinitiate připojení toohello Internetu. Pokud nechcete, aby byla tato připojení spustil, musí tato připojení toocreate tooblock pravidlo nebo vynutit vynucené tunelování.

#### <a name="system-routes-and-user-defined-routes"></a>Systémové trasy a trasy definované uživatelem

Když přidáte virtuální počítače (VM) tooa virtuální síť (VNet) v Azure, si všimnete hello virtuální počítače automaticky se možné toocommunicate mezi sebou přes síť hello. Není nutné toospecify bránu, i když hello virtuální počítače jsou v různých podsítích.

Hello totéž platí pro komunikaci z virtuálních počítačů toohello hello veřejného Internetu a dokonce tooyour do místní sítě při hybridní připojení z Azure tooyour vlastní datacenter je k dispozici.

![Systémové trasy](media/azure-network-security/azure-network-security-fig-9.png)

Tento tok komunikace je možné, protože Azure pomocí řady systémových tras toodefine toky provozu IP. Systémové trasy řídí tok hello komunikace v hello následující scénáře:

-   V nástroji hello stejné podsíti.

-   Z podsítě tooanother v rámci virtuální sítě.

-   Z virtuálních počítačů toohello Internetu.

-   Z virtuální síť tooanother virtuální sítě prostřednictvím brány sítě VPN.

-   Z virtuální sítě tooanother virtuální sítě prostřednictvím sítě VNet partnerského vztahu ([služby řetězení](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)).

-   Z virtuální sítě tooyour místní sítě prostřednictvím brány sítě VPN.

Mnoho podniků má striktní zabezpečení a dodržování předpisů požadavky, které vyžadují místní kontrola všechny pakety sítě tooenforce konkrétní zásady. Azure poskytuje mechanismus názvem [vynuceného tunelování](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-forced-tunneling) který směruje provoz z hello tooon místní virtuální počítače tak, že vytvoříte vlastní trasy nebo podle [protokol BGP (Border Gateway)](https://docs.microsoft.com/windows-server/remote/remote-access/bgp/border-gateway-protocol-bgp) oznámení o inzerovaném programu prostřednictvím ExpressRoute nebo VPN.

Vynucené tunelování v Azure je nakonfigurován pomocí virtuální síti trasy definované uživatelem (UDR). Přesměrování provozu tooan místní lokality je vyjádřeno jako brána Azure VPN toohello výchozí trasu.

Hello následující části jsou uvedeny hello aktuální omezení trasy a hello směrovací tabulky pro virtuální síť Azure:

-   Každá podsíť virtuální sítě má integrovanou, směrovací tabulky systému. Hello systémovou tabulku směrování má hello následující tři skupiny tras:

 -  **Místní virtuální síť trasy:** přímo toohello cílové virtuální počítače v hello stejné virtuální síti

 - **Pro místní trasy:** toohello Azure VPN gateway

 -  **Výchozí trasu:** přímo toohello Internetu. Jsou pakety určené toohello soukromé IP adresy není pokrytá hello předchozí dva trasy vyřadit.

-   S vydáním hello trasy definované uživatelem můžete vytvořit směrovací tabulku tooadd výchozí trasu a pak přidružit hello směrování tabulky tooyour VNet podsíť tooenable vynucené tunelování na těchto podsítí.

-   Je nutné tooset "výchozí web" mezi hello mezi různými místy místní lokality připojené toohello virtuální sítě.

-   Vynucené tunelování musí být přidruženy k virtuální síti, která má dynamické směrování brána sítě VPN (není statická brána).

- ExpressRoute vynuceného tunelování přes tento mechanismus není nakonfigurovaná, ale místo toho je ve inzeruje výchozí trasu přes hello ExpressRoute BGP povolený pro relace partnerského vztahu.

> [!Note]
> Další informace najdete v tématu hello [dokumentace ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) Další informace.

#### <a name="network-security-appliances"></a>Sítě zabezpečovací zařízení
Při skupiny zabezpečení sítě a trasy definované uživatelem může poskytnout míru zabezpečení sítě v hello sítě a transportní vrstvy hello [OSI model](https://en.wikipedia.org/wiki/OSI_model), budou toobe situace, kdy je potřebujete nebo tooenable zabezpečení s vyšší úrovní hello síťového zásobníku. V takových situacích doporučujeme nasadit virtuální sítě zabezpečovací zařízení poskytovaných Azure partnery.

![Sítě zabezpečovací zařízení](./media/azure-network-security/azure-network-security-fig-10.png)

Síť Azure zabezpečovací zařízení zlepšit zabezpečení virtuální sítě a síťových funkcí a jsou k dispozici od více dodavatelů prostřednictvím hello [Azure Marketplace](https://azuremarketplace.microsoft.com). Tyto virtuální zabezpečovací zařízení může být nasazený tooprovide:

-   Vysoce dostupné brány firewall

-   Prevence vniknutí

-   Zjišťování neoprávněných vniknutí

-   Brány firewall systému webové aplikace (WAFs)

-   Optimalizace sítě WAN

-   Směrování

-   Vyrovnávání zatížení

-   Síť VPN

-   Správa certifikátů

-   Active Directory

-   Vícefaktorové ověřování

#### <a name="application-gateway"></a>Application Gateway

[Microsoft Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) je vyhrazené virtuální zařízení poskytující doručení řadič aplikace (ADC) jako služba.

 ![Application Gateway](./media/azure-network-security/azure-network-security-fig-11.png)

Aplikační brána umožňuje toooptimize webové farmy výkon a dostupnost přesměrováním zátěže procesoru náročné SSL ukončení toohello Aplikační brána (snižování zátěže-protokolu SSL). Poskytuje také další vrstvy 7 směrování funkcí, včetně:

-   Kruhové dotazování distribuce příchozí provoz

-   Spřažení na základě souboru cookie relace

-   Na základě cestu směrování adres URL

-   Možnost toohost více webů za jeden Application Gateway


A [brány firewall webových aplikací (firewall webových aplikací)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) je k dispozici jako součást hello aplikační brány. To poskytuje ochranu tooweb aplikací z běžných chyb zabezpečení webové a zneužití. Application Gateway můžete nakonfigurovat jako internetovou bránu, interní pouze brány nebo obojí.

Aplikace brány firewall webových aplikací můžete spustit v režimu zjišťování nebo prevence. Běžné případ použití je správci toorun detekce režimu tooobserve provozu pro škodlivé vzorce. Po zjištění potenciálních zneužití měnící tooprevention režimu blokuje podezřelé příchozí přenosy.

 ![Application Gateway](./media/azure-network-security/azure-network-security-fig-12.png)

Kromě toho aplikace brány firewall webových aplikací umožňuje monitorovat webové aplikace před útoky pomocí protokolu v reálném čase firewall webových aplikací, které jsou integrovány s [Azure monitorování](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) a [Azure Security Center](https://azure.microsoft.com/services/security-center/) výstrahy tootrack firewall webových aplikací a snadno monitorovat trendy.

formátovaný protokolu JSON Hello přejde přímo toohello zákazníkův účet úložiště. Mít plnou kontrolu nad tyto protokoly a můžete použít vlastní zásady uchovávání informací.

Tyto protokoly můžete také ingestování do vlastní analytics systém pomocí [integrace se službou Azure protokolu](https://aka.ms/AzLog). Protokoly firewall webových aplikací taky jsou integrované s [Operations Management Suite (OMS)](https://www.microsoft.com/cloud-platform/operations-management-suite) abyste je mohli používat analýzy protokolů OMS tooexecute pokročilé podrobných dotazy.

#### <a name="azure-web-application-firewall-waf"></a>Brány firewall Azure webových aplikací (firewall webových aplikací)

Webové aplikace jsou stále cíle nebezpečné útoky, které využívají běžné známých chyb zabezpečení, například vkládání SQL, křížové útoky skriptování a další útoky, které se zobrazují v hello [OWASP prvních 10](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project). Zabránění takové zneužití v aplikaci hello vyžaduje přísných Údržba, opravy a monitorování v několika vrstev topologie aplikace hello.

 ![Brány Firewall Azure webových aplikací (firewall webových aplikací)](./media/azure-network-security/azure-network-security-fig-13.png)

Centralizované [brány firewall webových aplikací (firewall webových aplikací)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) může chránit proti útokům na web a zjednodušuje správu zabezpečení bez nutnosti změny aplikace.

Řešení firewall webových aplikací můžete rovněž reagovat ohrožení zabezpečení tooa rychlejší podle opravy známých ohrožení zabezpečení do centrálního umístění a zabezpečení těchto jednotlivých webových aplikací. Existující application Gateway může být snadno převedený tooa webové aplikace povolena brána firewall aplikační brány.

#### <a name="network-availability-controls"></a>Ovládací prvky dostupnost sítě

Existují různé možnosti toodistribute síťového provozu pomocí Microsoft Azure. Tyto možnosti fungují různě, obsahují jiné sady funkcí a podporují různé scénáře. Lze je používat jednotlivě nebo je kombinovat.

Následující jsou hello řízení dostupnosti sítě:

-   Nástroj pro vyrovnávání zatížení Azure

-   Application Gateway

-   Traffic Manager

**Azure nástroj pro vyrovnávání zatížení**

Zajišťuje vysoké dostupnosti a výkonu v síti tooyour aplikace. Je Vyrovnávání zatížení vrstvy 4 (TCP, UDP), která distribuuje příchozí komunikaci mezi pořádku instancí služby definované v sadě s vyrovnáváním zatížení.

 ![Nástroj pro vyrovnávání zatížení Azure](media/azure-network-security/azure-network-security-fig-14.png)


Azure nástroj pro vyrovnávání zatížení můžete nakonfigurovat:

-   Vyrovnávat zatížení příchozí internetové přenosy toovirtual počítačů. Tato konfigurace se označuje jako [Vyrovnávání zatížení internetového](https://docs.microsoft.com/azure/load-balancer/load-balancer-internet-overview).

-   Přenosy Vyrovnávání zatížení mezi virtuálními počítači ve virtuální síti, mezi virtuálními počítači v cloudové služby nebo mezi místními počítači a virtuální počítače ve virtuální síti mezi různými místy. Tato konfigurace se označuje jako [Vyrovnávání zatížení pro vnitřní](https://docs.microsoft.com/azure/load-balancer/load-balancer-internal-overview).

-   Předat dál externích přenosů tooa konkrétní virtuální počítač.

Všechny prostředky v cloudu hello potřebovat dosažitelný z Internetu hello veřejné toobe adres IP. Hello cloudové infrastruktury v Azure používá směrovat IP adresy pro její prostředky. Azure používá překlad síťových adres (NAT) s veřejné IP adresy toocommunicate toohello Internetu.

 **Aplikační brány**

 Application Gateway pracuje na aplikační vrstvu hello (7 vrstvy v zásobníku odkaz sítě OSI hello). Jedná jako reverznímu proxy serveru služby, se ukončuje připojení klienta hello a předávání požadavků koncové body tooback-end.

 **Správce provozu**

Microsoft Azure Traffic Manager umožňuje toocontrol hello distribuce provozu generovaného uživateli pro koncové body služby v různých datových centrech. Koncové body služby, které jsou podporovány nástrojem Traffic Manager zahrnují virtuálních počítačích Azure, webové aplikace a cloudových služeb. Službu Traffic Manager můžete používat také s externími koncovými body mimo Azure.

Traffic Manager používá hello systému DNS (Domain Name) toodirect klient požádá o toohello nejvhodnější koncového bodu, na základě [metodu směrování provozu](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods) a stavu hello hello koncových bodů. Traffic Manager poskytuje řadu směrování provozu metody toosuit jinou aplikaci potřeby, koncový bod stavu [monitorování](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring)a automatické převzetí služeb při selhání. Správce provozu je odolný toofailure, včetně hello selhání celé oblasti Azure.

Azure Traffic Manager umožňuje toocontrol hello distribuce přenosů mezi koncových bodů vaší aplikace. Koncový bod je všechny internetové služby hostované uvnitř nebo mimo Azure.

Správce provozu přináší dvě klíčové výhody:

-   Distribuce přenosů podle tooone několik [metody směrování provozu](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods).

-   [Nepřetržité monitorování stavu koncový bod](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring) a automatické převzetí služeb při selhání koncových bodů.

Když se klient pokusí tooconnect tooa služby, se musí nejprve přeložit název DNS hello hello služby tooan IP adresy. Klient Hello se pak připojí toothat IP adresu tooaccess hello služby. Koncové body na základě pravidel hello hello metody směrování provozu Traffic Manager používá DNS toodirect klienti toospecific služby. Klienti připojovat přímo toohello vybrané koncový bod. Traffic Manager není serveru proxy nebo brána. Správce provozu nezná hello provoz předávání mezi hello klientem a službou hello.

### <a name="azure-network-validation"></a>Ověření síť Azure

Síť Azure ověření je tooensure, který hello síť Azure pracuje, jako je nakonfigurován a lze provést ověření pomocí hello služby a funkce dostupné toomonitor hello sítě. Sledovací proces sítě Azure, dostanete nadbytku protokolování a diagnostické funkce, které můžete umožnit příslušným insights toounderstand stav a výkon sítě. Tyto možnosti jsou dostupné přes portál, prostředí PowerShell, rozhraní příkazového řádku, Rest API a sady SDK.

Zabezpečení provozu Azure odkazuje toohello služeb, ovládací prvky a funkce dostupné toousers pro ochranu svá data, aplikace a dalších prostředků ve službě Microsoft Azure. Zabezpečení provozu Azure je založený na rozhraní, které zahrnuje hello poznatky získané při různých možnostech, které jsou jedinečné tooMicrosoft, včetně hello Microsoft SDL Security Development Lifecycle (), hello Centre odpovědi zabezpečení Microsoft program a hloubkové povědomí o hello kybernetického zabezpečení threat na šířku.

-   [Azure Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview)

-   [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)

-   [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview)

-   [Azure sledovací proces sítě](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)

-   [Azure Storage analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)

-   Azure Resource Manager

#### <a name="azure-resource-manager"></a>Azure resource Manageru

Hello osoby a procesy, které provozují Microsoft Azure jsou možná hello nejdůležitější funkce zabezpečení hello platformy. Tato část popisuje funkce společnosti Microsoft globální infrastruktuře datacenter, které pomůžou vylepšit a spravovat zabezpečení, kontinuitu a ochrany osobních údajů.

Hello infrastrukturu aplikace obvykle tvoří celá řada komponent, může být virtuální počítač, účet úložiště a virtuální sítě, nebo webové aplikace, databáze, databázový server a služby třetích stran. Tyto komponenty nevidíte jako samostatné entity, ale jako související a vzájemně provázané části jedné entity. Chcete toodeploy, spravovat a monitorovat jako skupinu. Azure Resource Manager umožňuje toowork s hello prostředky ve vašem řešení jako se skupinou.

Můžete nasadit, aktualizovat nebo odstranit všechny hello prostředky pro vaše řešení v rámci jediné koordinované operace. Pro nasazení použijete šablonu a tato šablona může fungovat v různých prostředích, jako například v testovacím, přípravném nebo produkčním prostředí. Resource Manager poskytuje zabezpečení, auditování a označování funkce toohelp spravovat prostředky po nasazení.

**Hello výhody použití Resource Manager**

Resource Manager poskytuje několik výhod:

-   Můžete nasadit, spravovat a monitorovat všechny hello prostředky pro vaše řešení jako skupina, nikoli zpracovávat jednotlivě.

-   Můžete opakovaně nasadit řešení v celém hello životního cyklu a mít přitom jistotu, že vaše prostředky jsou nasazeny v konzistentním stavu.

-   Infrastrukturu můžete spravovat pomocí deklarativních šablon místo skriptů.

-   Můžete definovat hello závislosti mezi prostředky, takže se nasadí ve správném pořadí hello.

-   Služby tooall řízení přístupu můžete použít ve vaší skupině prostředků, protože do platformy pro správu hello je nativně integrováno řízení přístupu na základě Role (RBAC).

-   Můžete použít značky tooresources toologically uspořádat všechny prostředky hello ve vašem předplatném.

-   Zobrazením náklady pro skupinu prostředků, které sdílejí značky můžete zpřehlednit fakturaci vaší organizace.

> [!Note]
> Resource Manager poskytuje nový způsob toodeploy a správy vašich řešení. Pokud jste použili hello dřívější model nasazení a chcete toolearn o změnách hello naleznete [nasazení Resource Manager principy a nasazení classic](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model).

## <a name="azure-network-logging-and-monitoring"></a>Síť Azure protokolování a monitorování

Azure nabízí mnoho toomonitor nástroje, zabránit, zjistit a reagovat toonetwork události zabezpečení. Mezi hello nejúčinnějších nástrojů k dispozici tooyou v této oblasti patří:

-   Network Watcher

-   Úrovně monitorování sítě prostředků

-   Log Analytics

### <a name="network-watcher"></a>Sledovací proces sítě

[Sledovací proces sítě](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) – na základě scénáře monitorování se poskytuje s funkcí hello v sledovací proces sítě. Tato služba obsahuje zachytáváním paketů, další směrování, IP tok ověření, zobrazení skupiny zabezpečení, tok protokolů NSG. Scénář úrovně monitorování obsahuje zobrazení tooend end síťovým prostředkům v monitorování kontrast tooindividual sítě prostředků.

 ![Network Watcher](./media/azure-network-security/azure-network-security-fig-15.png)

Sledovací proces sítě je místní služba, která umožňuje vám toomonitor a diagnostikovat podmínky na úrovni scénář sítě, do a z Azure. Diagnostika sítě a k dispozici sledovací proces sítě vizualizace nástroje vám pomůžou pochopit, diagnostikovat a získat statistiky tooyour sítě v Azure.

Sledovací proces sítě má aktuálně hello následující možnosti:

#### <a name="topology"></a>topologie

[Topologie](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview) vrátí graf síťovým prostředkům ve virtuální síti. Hello graf znázorňuje hello propojení mezi hello prostředky toorepresent hello end tooend připojení k síti. Topologie hello portálu, vrátí objektů prostředků hello na podle základ virtuální sítě. vztahy Hello, jsou použité v ukázkách linkami mezi prostředky hello mimo oblast hello sledovací proces sítě, i v případě, že v hello prostředku skupiny se nezobrazí. Hello prostředky, vrátí se v zobrazení portálu hello jsou podmnožinou hello síťové součásti, které vykreslovacích. hello úplný seznam toosee síťových prostředků, můžete použít [prostředí PowerShell](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-powershell) nebo [REST](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-rest).

Jako prostředky jsou vráceny hello připojení mezi jejich jsou modelovat v dva vztahy.

- **Členství ve skupině** -virtuální síť obsahuje podsíť, která obsahuje síťový adaptér.

- **Související** – síťový adaptér A je přidružený virtuální počítač.

#### <a name="variable-packet-capture"></a>Zachytáváním paketů proměnné

Sledovací proces sítě [zachytáváním paketů proměnné](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) můžete zaznamenat toocreate paketů relace tootrack tooand na provoz z virtuálního počítače. Zachytáváním paketů pomáhá sítě anomálií toodiagnose obou reaktivně a proactivity. Mezi další použití patří shromažďování statistiku sítě, získá informace o síti vniknutí, toodebug klient server komunikace a mnoho dalšího.

Zachytáváním paketů je rozšíření virtuálního počítače, který se spustil vzdáleně přes sledovací proces sítě. Tato funkce snižuje zátěž hello spuštěných zachytáváním paketů ručně na hello potřeby virtuální počítač, který úspora času. Zachytáváním paketů můžete spustit prostřednictvím portálu hello, prostředí PowerShell, rozhraní příkazového řádku nebo REST API. Je jeden příklad, jak můžete spustit zachytáváním paketů s výstrahami virtuálního počítače.

#### <a name="ip-flow-verify"></a>Ověřte toku IP

[Ověřte IP toky](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) zkontroluje, jestli je paket povolen nebo odepřen tooor z virtuálního počítače na základě informací o 5 řazené kolekce členů. Tyto informace se skládá z směr, protokol, místní IP, vzdálené IP, místního portu a vzdáleného portu. Pokud paket hello je zakázané skupiny zabezpečení, je vrácen název hello hello pravidlo, které odepřen hello paketů. Zatímco můžete vybrat všechny zdrojové i cílové adresy IP, tato funkce vám pomůže rychle diagnostikovat problémy s připojením z nebo toohello správci internet a z nebo toohello v místním prostředí.

Toky IP ověřte cílem síťové rozhraní virtuálního počítače. Tok přenosů dat je pak ověřit podle hello nakonfigurované nastavení tooor z rozhraní sítě. Tato možnost je užitečná při potvrzení, zda pravidla v skupinu zabezpečení sítě neblokuje inzerování vstupních nebo výstupních tooor provoz z virtuálního počítače.

#### <a name="next-hop"></a>Další směrování

Určuje hello [další segment](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) pro pakety směrovány v hello prostředky infrastruktury sítě Azure, všechny povolení toodiagnose je špatně nakonfigurovaný trasy definované uživatelem. Přenosy z virtuálního počítače se budou odesílat tooa cíl v závislosti na hello účinné postupy spojené s síťový adaptér. Další směrování získá hello typ dalšího směrování a IP adresa z paketu z konkrétní virtuální počítač a síťový adaptér. To pomáhá toodetermine, pokud hello paketů se směrovanou toohello cílové nebo je dírkového hello provoz probíhá černé.

Další směrování také vrátí hodnotu hello směrovací tabulce přidružené k dalším místě směrování hello. Při dotazování další směrování, pokud trasu hello je definován jako trasy definované uživatelem, bude vrácen danou trasu. V opačném případě vrátí další segment "Systémová trasa".

#### <a name="security-group-view"></a>zobrazení skupiny zabezpečení

Získá hello zabezpečení efektivní a použitých pravidel, která se použijí na virtuálním počítači. Skupiny zabezpečení sítě na úrovni podsítě nebo na úrovni seskupování souvisejí. Když přidružené na úrovni podsítě, bude se vztahovat tooall hello instance virtuálních počítačů v podsíti hello. Síť [zobrazení skupiny zabezpečení](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview) vrátí všechny hello nakonfigurované skupiny Nsg a pravidel, které jsou přidružené úrovni síťových Adaptérů a podsítě pro virtuální počítač poskytuje přehled o konfiguraci hello. Kromě toho pravidla efektivní zabezpečení hello se vrátí pro každou hello síťových adaptérů ve virtuálním počítači. Zobrazení pomocí skupiny zabezpečení sítě, můžete vyhodnotit virtuální počítač chyb zabezpečení sítě, jako je například otevřené porty. Můžete také ověřit, pokud vaše skupina zabezpečení sítě funguje podle očekávání, na základě [porovnání mezi hello nakonfigurované a hello pravidla efektivní zabezpečení](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-auditing-powershell).

#### <a name="nsg-flow-logging"></a>Protokolování toku NSG

 Tok protokoly pro skupinu zabezpečení sítě povolte protokoly toocapture související se tootraffic, které jsou povolené nebo zakázané pravidla zabezpečení hello ve skupině hello. tok Hello je definována informací o 5-n-tice – zdrojové adresy IP, cílovou IP adresu, zdrojový Port, cílový Port a protokol.

[Skupina zabezpečení sítě toku protokoly](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) jsou funkce sledovací proces sítě, který vám umožní tooview informace o příchozí a odchozí provoz IP prostřednictvím skupinu zabezpečení sítě.

#### <a name="virtual-network-gateway-and-connection-troubleshooting"></a>Brána virtuální sítě a odstraňování problémů s připojením

Sledovací proces sítě obsahuje řadu funkcí, protože se týká toounderstanding síťovým prostředkům v Azure. Jeden z těchto funkcí je prostředek řešení potíží. [Řešení potíží s prostředků](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest) nelze volat pomocí prostředí PowerShell, rozhraní příkazového řádku nebo REST API. Při volání, sledovací proces sítě kontroluje stav hello bránu virtuální sítě nebo připojení a vrátí nalezených výsledcích.

Tato část vás provede úlohy hello různých správy, které jsou aktuálně dostupné pro řešení potíží s prostředků.

-   [Řešení potíží s bránu virtuální sítě](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest)

-   [Vyřešte potíže připojením](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest)

#### <a name="network-subscription-limits"></a>Limity předplatného sítě

[Sítě limity předplatného](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) poskytují podrobnosti o využití hello jednotlivých hello síťovému prostředku v rámci předplatného v oblasti proti hello maximální počet prostředků, které jsou k dispozici.

#### <a name="configuring-diagnostics-log"></a>Konfiguraci diagnostiky protokolu

Poskytuje sledovací proces sítě [diagnostické protokoly](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) zobrazení. Toto zobrazení obsahuje všechny síťové prostředky, které podporují protokolování diagnostiky. Z tohoto hlediska můžete povolit nebo zakázat síťových prostředků, snadno a rychle.

### <a name="network-resource-level-monitoring"></a>Úrovně monitorování sítě prostředků

Hello následující funkce jsou k dispozici pro monitorování na úrovni prostředků:

#### <a name="audit-log"></a>Protokol auditování

Operace provedené v rámci konfigurace hello sítí přihlášeni. Tyto protokoly auditu jsou nezbytné tooestablish různých compliances. Tyto protokoly můžete zobrazit v hello portál Azure nebo pomocí nástroje Microsoft, jako je Power BI nebo nástroje třetích stran. Protokoly auditu jsou k dispozici prostřednictvím portálu hello, prostředí PowerShell, rozhraní příkazového řádku a Rest API.

> [!Note]
> Další informace o protokolů auditu najdete v tématu [auditovat operace s Resource Managerem](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
Protokoly auditu jsou k dispozici pro operace udělat na všechny síťové prostředky.


#### <a name="metrics"></a>Metriky

Metriky jsou měření výkonu a čítače shromážděných za období. Metriky jsou aktuálně dostupné pro službu Application Gateway. Metriky lze použít tootrigger výstrahy podle prahovou hodnotu. Ve výchozím nastavení služba Azure Application Gateway monitoruje stav hello všechny prostředky v jeho fond back-end a automaticky odebere všechny prostředků z fondu hello považoval za poškozený. Aplikační brána pokračuje toomonitor hello není v pořádku instancí a přidá je zpět toohello pořádku fond back-end, jakmile budou k dispozici a reakce toohealth testy. Aplikační brána odešle sondy stavu hello s hello stejný port, který je definován v nastavení HTTP back-end hello. Tato konfigurace zajistí, že tento test hello je testování hello stejný port, že zákazníci by pomocí tooconnect toohello back-end.

> [!Note]
> V tématu [Application Diagnostics brány](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview) tooview jak metriky lze použít toocreate výstrahy.

#### <a name="diagnostic-logs"></a>Diagnostické protokoly

Pravidelné a spontánních události jsou vytvořené síťové prostředky a protokolovány v účtech úložiště, odeslané tooan centra událostí nebo analýzy protokolů. Tyto protokoly poskytují přehled o stavu hello prostředku. Tyto protokoly můžete zobrazit v nástrojů, jako je Power BI a analýzy protokolů. jak tooview diagnostické protokoly, navštivte toolearn [analýzy protokolů](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics).

Diagnostické protokoly jsou k dispozici pro [nástroj pro vyrovnávání zatížení](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log), [skupin zabezpečení sítě](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log), trasy a [Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics).

Sledovací proces sítě poskytuje že diagnostické protokoly zobrazení. Toto zobrazení obsahuje všechny síťové prostředky, které podporují protokolování diagnostiky. Z tohoto hlediska můžete povolit nebo zakázat síťových prostředků, snadno a rychle.

### <a name="log-analytics"></a>Log Analytics

[Analýza protokolu](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) je služba v [Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) , sleduje vaše cloudové a místní prostředí toomaintain jejich dostupnost a výkon. Shromáždí data generována prostředky ve vašich cloudových a místních prostředích a z dalších monitorování tooprovide analysis nástroje napříč více zdrojů.

Analýzy protokolů nabízí hello následující řešení pro monitorování vaší sítí:

-   Sledování výkonu sítě (NPM)

-   Analýza Azure Application Gateway

-   Skupina zabezpečení sítě Azure analytics

#### <a name="network-performance-monitor-npm"></a>Sledování výkonu sítě (NPM)
Hello [sledování výkonu sítě](https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor) řešení pro správu je síť řešení monitorování, které sleduje stav hello, dostupnosti a dostupnosti sítě.

Jedná se o použitých toomonitor připojení mezi:

-   veřejný cloud a místní

-   datových center a umístění uživatele (firemních pobočkách)

-   podsítě hostování různé úrovně víceúrovňových aplikací.


#### <a name="azure-application-gateway-analytics-in-log-analytics"></a>Analýza brány Azure aplikace v analýzy protokolů

Hello následující protokoly jsou podporovány pro Application Gateway:

-   ApplicationGatewayAccessLog

-   ApplicationGatewayPerformanceLog

-   ApplicationGatewayFirewallLog

pro Application Gateway se podporují Hello následující metriky:

-   propustnost 5 minut

#### <a name="azure-network-security-group-analytics-in-log-analytics"></a>Analytické údaje skupiny zabezpečení sítě Azure v analýzy protokolů

Hello tyto protokoly jsou podporovány pro [skupin zabezpečení sítě](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log):

- **NetworkSecurityGroupEvent:** obsahuje položky, u které skupina NSG použitá tooVMs a instance rolí na základě adresy MAC jsou pravidla. Hello stav pro tato pravidla se shromažďují každých 60 sekund.

- **NetworkSecurityGroupRuleCounter:** obsahuje položky pro jednotlivé skupiny NSG počet použití pravidla je použité toodeny nebo povolení provozu.

## <a name="next-steps"></a>Další kroky
Další informace o zabezpečení načtením některá témata s našimi podrobné zabezpečení:

-   [Analýzy protokolů pro skupinu zabezpečení sítě (Nsg)](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log)

-   [Sítě inovace této jednotky hello cloudu přerušení](https://azure.microsoft.com/blog/networking-innovations-that-drive-the-cloud-disruption/)

-   [SONiC: Síťový přepínač software, která pohání hello globální cloudu Microsoft hello](https://azure.microsoft.com/blog/sonic-the-networking-switch-software-that-powers-the-microsoft-global-cloud/)

-   [Jak Microsoft sestavení jeho rychlé a spolehlivé globální sítě](https://azure.microsoft.com/blog/how-microsoft-builds-its-fast-and-reliable-global-network/)

-   [Osvětlení až inovací sítě](https://azure.microsoft.com/blog/lighting-up-network-innovation/)
