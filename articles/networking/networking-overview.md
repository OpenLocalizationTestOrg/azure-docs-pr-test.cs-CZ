---
title: "sítě aaaAzure | Microsoft Docs"
description: "Další informace o Azure síťové služby a možnosti."
services: networking
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/19/2017
ms.author: jdial
ms.openlocfilehash: 18945d139427f2e65138c0fd223e663fa46e9211
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-networking"></a>Síť Azure

Azure poskytuje celou řadu možností sítě, které lze použít společně nebo samostatně. Kliknutím na jakýkoli z následujících klíčových funkcí toolearn více o nich hello:
- [Připojení mezi prostředky Azure](#connectivity): prostředky Azure připojit společně v zabezpečené, privátní virtuální sítě v cloudu hello.
- [Připojení k Internetu](#internet-connectivity): komunikují přes hello Internet tooand z prostředků Azure.
- [Místní připojení](#on-premises-connectivity): připojení místní síti tooAzure prostředků prostřednictvím virtuální privátní sítě (VPN) přes hello Internet nebo prostřednictvím tooAzure vyhrazené připojení.
- [Načíst vyrovnávání a provoz směrem](#load-balancing): tooservers provoz Vyrovnávání zatížení v hello stejné umístění a tooservers přímé přenosy v různých umístěních.
- [Zabezpečení](#security): filtrování síťového provozu mezi podsítě v síti nebo jednotlivé virtuální počítače (VM).
- [Směrování](#routing): použijte výchozí směrování nebo plnou kontrolu nad směrování mezi vaší Azure a místní prostředky.
- [Možnosti správy](#manageability): monitorování a správě síťových prostředků Azure.
- [Nástroje pro nasazení a konfigurace](#tools): použití webový portál nebo nástroje příkazového řádku pro různé platformy toodeploy a konfiguraci síťových prostředků.

## <a name="Connectivity"></a>Připojení mezi prostředky Azure

Prostředky Azure jako virtuální počítače, cloudové služby, sady škálování virtuálních počítačů a prostředí Azure App Service můžete vzájemně komunikovat soukromě přes virtuální síť Azure (VNet). Virtuální síť je to logická izolace cloudu Azure vyhrazeného tooyour hello [předplatné](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fnetworking%2ftoc.json). Můžete implementovat více virtuálních sítí v rámci každé předplatné Azure a Azure [oblast](https://azure.microsoft.com/regions). Každý virtuální sítě je izolovaná od jiných virtuálních sítí. Pro každý virtuální síť můžete:

- Zadejte vlastní prostor privátní IP adresy pomocí veřejné a privátní adresy (RFC 1918). Azure přiřadí připojené toohello prostředky virtuální sítě privátní IP adresy z hello adresního prostoru, který přiřadíte.
- Segment hello virtuální sítě do jedné nebo více podsítí a přidělovat část podsíť tooeach hello virtuální síť adres místa.
- Můžete použít Azure překlad nebo zadejte že vlastní server DNS za účelem použití prostředků připojený tooa virtuální sítě.

Další informace o hello, čtení hello služby Azure Virtual Network toolearn [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) článku. Virtuální sítě tooeach další se můžete připojit, povolení prostředky toocommunicate tooeither virtuální síť připojení mezi sebou mezi virtuálními sítěmi. Můžete použít jednu nebo obě následující možnosti tooconnect virtuálních sítí tooeach jiných hello:

- **Partnerský vztah:** umožňuje prostředky připojené toodifferent sítě Azure Vnet v rámci hello stejné oblasti Azure toocommunicate mezi sebou. Hello šířky pásma a latence napříč hello virtuální sítě je hello stejné jako v případě, že hello prostředky byly připojené toohello stejnou virtuální síť. toolearn Další informace o vytvoření partnerského vztahu, přečtěte si hello [partnerského vztahu Přehled virtuálních sítí](../virtual-network/virtual-network-peering-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) článku.
- **Brána sítě VPN:** umožňuje prostředky připojené toodifferent sítě Azure Vnet v rámci různých oblastech Azure toocommunicate mezi sebou. Provoz mezi virtuálními sítěmi toky prostřednictvím služby Azure VPN Gateway. Šířky pásma mezi virtuálními sítěmi je omezená toohello hello brány. Další informace o propojení virtuálních sítí s brána sítě VPN, přečtěte si hello toolearn [konfigurace připojení typu VNet-to-VNet v oblastech](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) článku.

## <a name="internet-connectivity"></a>Připojení k Internetu

Všechny prostředky Azure připojené tooa virtuální síť mít toohello odchozí připojení k Internetu ve výchozím nastavení. Hello privátní IP adresu prostředku hello je, že zdrojová síťová adresa přeložen hello infrastrukturu Azure (překládat pomocí SNAT) tooa veřejnou IP adresu. Další informace o odchozí připojení k Internetu, přečtěte si hello toolearn [pochopení odchozí připojení v Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fnetworking%2ftoc.json) článku.

toocommunicate příchozí tooAzure prostředky z hello Internet nebo toocommunicate odchozí toohello, které Internetu bez překládat pomocí SNAT, prostředek musí být přiřazen veřejnou IP adresu. Další informace o veřejné IP adresy, přečtěte si hello toolearn [veřejné IP adresy](../virtual-network/virtual-network-public-ip-address.md?toc=%2fazure%2fnetworking%2ftoc.json) článku.

## <a name="on-premises-connectivity"></a>Místní připojení

Prostředky ve vaší virtuální síti získat přístup bezpečně prostřednictvím připojení k síti VPN nebo přímé privátní připojení. toosend síťový provoz mezi virtuální sítě Azure a v místní síti, musíte vytvořit bránu virtuální sítě. Můžete nakonfigurovat nastavení pro hello brány toocreate hello typ připojení, které chcete, VPN nebo ExpressRoute.

Můžete připojit vaše místní síť tooa sítě VNet pomocí libovolné kombinace hello následující možnosti:

**Point-to-site (VPN prostřednictvím protokolu SSTP)**

Hello následující obrázek ukazuje samostatné toosite bodu připojení mezi více počítačů a virtuální sítě:

![Point-to-Site](./media/networking-overview/point-to-site.png)

Toto připojení mezi jednoho počítače a virtuální sítě. Tento typ připojení je skvělé, pokud jste se právě Začínáme s Azure, nebo pro vývojáře, protože vyžaduje žádné nebo téměř žádné změny tooyour stávající síť. Je také vhodné, pokud se připojujete ze vzdáleného umístění, jako je například z konference nebo domovský. Připojení point-to-site jsou často kombinaci s připojení site-to-site pomocí hello stejné brány virtuální sítě. připojení Hello používá hello SSTP protokol tooprovide zašifrovaná komunikace přes Internet hello mezi počítačem hello a hello virtuální sítě. Hello latence pro síť VPN point-to-site nepředvídatelným, protože hello provoz prochází hello Internetu.

**Site-to-site (protokolu IPsec/IKE tunelového připojení sítě VPN)**

![Site-to-Site](./media/networking-overview/site-to-site.png)

Toto připojení mezi místní zařízení VPN a služby Azure VPN Gateway. Tento typ připojení umožňuje žádné místnímu prostředku, abyste autorizovali tooaccess hello virtuální sítě. je Hello připojení VPN pomocí protokolu IPSec/IKE, která poskytuje šifrovanou komunikaci přes Internet hello mezi místní zařízení a hello Azure VPN gateway. Můžete připojit více místními lokalitami toohello stejnou bránou sítě VPN. Hello místní zařízení VPN v každé lokalitě musí mít externě směřujících veřejnou IP adresu, která není za adres (NAT) Hello latence pro připojení site-to-site nepředvídatelným, protože hello provoz prochází hello Internetu.

**ExpressRoute (vyhrazené soukromé připojení)**

![ExpressRoute](./media/networking-overview/expressroute.png)

Tento typ připojení je navázat mezi vaší sítí a Azure prostřednictvím partnerem ExpressRoute. Toto připojení je soukromé. Provoz neprochází hello Internetu. Hello latence pro připojení typu ExpressRoute je předvídatelný, vzhledem k tomu, že provoz není procházení hello Internetu. ExpressRoute je možné kombinovat s připojení site-to-site.

Další informace o všech hello předchozí možnosti připojení, přečtěte si hello toolearn [diagramy topologie připojení](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fnetworking%2ftoc.json) článku.

## <a name="load-balancing"></a>Načíst vyrovnávání a provoz směrem

Microsoft Azure poskytuje několik služeb pro správu, jak je distribuován síťový provoz a skupinu s vyrovnáváním zatížení. Můžete použít některou z následujících možností, samostatně nebo společně hello:

**Vyrovnávání zatížení DNS**

Hello služba Azure Traffic Manager poskytuje globální Vyrovnávání zatížení DNS. Správce provozu odpoví tooclients s IP adresou hello pořádku koncového bodu, na základě jedné z následujících metod směrování hello:
- **Zeměpisná:** klienti jsou směrované na základě toospecific koncových bodů (Azure, externí nebo vnořená), na které zeměpisné umístění svého dotazu DNS pochází z. Tato metoda umožňuje scénáře, kde je důležité zároveň budete vědět, geografické oblasti klienta a směrování je podle něj. Mezi příklady patří soulad s pověřeními suverenity dat, lokalizace obsah & uživatelské prostředí a měření provoz z různých oblastech.
- **Výkon:** hello IP adresu vrátil toohello klienta je hello "nejbližší" toohello klienta. Hello 'nejbližší' koncový bod není nutně nejbližší měřený podle zeměpisného vzdálenost. Místo toho tato metoda určuje nejbližší koncový bod hello měřením latence sítě. Správce provozu udržuje Internetu latence tabulky tootrack hello času jejich návratu mezi rozsahů IP adres a každé datové centrum Azure.
- **Priorita:** provoz je řízené toohello primární koncový bod (nejvyšší priorita). Pokud hello primární koncový bod není k dispozici, směrování Traffic Manager hello druhý koncový bod toohello provoz. Pokud oba hello primární a sekundární koncových bodů nejsou k dispozici, provoz hello přejde toohello třetí, a tak dále. Dostupnost hello koncového bodu je založena na stav hello nakonfigurované (povolit nebo zakázat) a hello monitorování probíhající koncového bodu.
- **Vážené kruhové dotazování:** pro každý požadavek Traffic Manager náhodně vybere koncový bod k dispozici. pravděpodobnost Hello podle uvážení koncového bodu je založená na hello váhu přiřazené tooall koncové body k dispozici. Pomocí hello mezi všechny koncové body výsledky v distribuce přenosů i vážené stejné. Pomocí vyšší nebo nižší váhu na konkrétním koncovým bodům způsobí, že tyto koncové body toobe více nebo méně často, vrátí se v odpovědi DNS hello.

Hello následující obrázek ukazuje žádost webové aplikace směrované tooa koncový bod webové aplikace. Koncové body může být také jinými službami Azure, jako je například virtuální počítače a cloudové služby.

![Traffic Manager](./media/networking-overview/traffic-manager.png)

Hello klient připojí přímo toothat koncový bod. Azure Traffic Manager zjistí, že koncový bod není v pořádku a pak přesměruje klienti tooa v pořádku, jiný koncový bod. více o Traffic Manager číst hello toolearn [přehled Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) článku.

**Vyrovnávání zatížení aplikace**

Hello služby Azure Application Gateway poskytuje aplikace doručení řadiče (ADC) jako služba. Aplikační brána nabízí různé vrstvy 7 (HTTP či HTTPS) služby Vyrovnávání zatížení schopnosti pro vaše aplikace, včetně webovou aplikaci brány firewall tooprotect vaší webové aplikace na ohrožení zabezpečení a zneužití. Aplikační bránu můžete taky toooptimize webové farmy produktivitu přesměrováním náročná na prostředky procesoru SSL ukončení toohello aplikační brány. 

Další možnosti směrování vrstvy 7 zahrnují kruhového dotazování distribuce příchozí provoz, spřažení na základě souboru cookie relace, na základě cestu směrování adres URL a hello možnost toohost více webů za bránu jednu aplikaci. Application Gateway můžete nakonfigurovat jako bránu internetové brány, bránu pouze interní nebo obojí. Application Gateway je plně Azure spravované, škálovatelné a vysoce dostupné. Nabízí celou řadu možností diagnostiky a protokolování, které zlepšují správu. Další informace o Application Gateway, přečtěte si hello toolearn [Application Gateway přehled](../application-gateway/application-gateway-introduction.md?toc=%2fazure%2fnetworking%2ftoc.json) článku.

Hello následující obrázek znázorňuje URL cesty založené na směrování s aplikační brány:

![Application Gateway](./media/networking-overview/application-gateway.png)

**Vyrovnávání zatížení sítě**

Hello Vyrovnávání zatížení Azure poskytuje vysoce výkonné, nízkou latencí 4 vrstvy Vyrovnávání zatížení pro všechny protokoly UDP a TCP. Spravuje příchozí a odchozí připojení. Můžete nakonfigurovat veřejný a interní Vyrovnávání zatížení sítě koncové body. Můžete definovat pravidla toomap příchozí připojení tooback-end fondu cíle pomocí TCP a HTTP zjišťování stavu možnosti toomanage dostupnost služeb. Další informace o vyrovnávání zatížení, přečtěte si hello toolearn [přehled nástroje pro vyrovnávání zatížení](../load-balancer/load-balancer-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) článku.

Hello následující obrázek ukazuje internetového vícevrstvé aplikace, která využívá i nástroje pro vyrovnávání zatížení externí i interní:

![Nástroj pro vyrovnávání zatížení](./media/networking-overview/load-balancer.png)

## <a name="security"></a>Zabezpečení

Můžete filtrovat provoz tooand z prostředků Azure pomocí hello následující možnosti:

- **Síť:** můžete implementovat síť Azure zabezpečení skupiny (Nsg) toofilter příchozí a odchozí provoz tooAzure prostředky. Každá skupina NSG obsahuje jedno nebo více pravidel příchozí a odchozí. Každé pravidlo určuje hello zdrojové IP adresy, cílové IP adresy, portu a protokolu, který je provoz filtrovaný pomocí. Skupiny Nsg může být použité tooindividual podsítě a jednotlivé virtuální počítače. Další informace o skupin Nsg, přečtěte si hello toolearn [přehled skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md?toc=%2fazure%2fnetworking%2ftoc.json) článku.
- **Aplikace:** pomocí služby Application Gateway s brány firewall webových aplikací můžete chránit vaše webové aplikace z ohrožení zabezpečení a zneužití. Běžných příkladů jsou SQL prostřednictvím injektáže skriptování mezi weby a poškozené záhlaví. Aplikační brána filtruje tento provoz a zastaví dosáhly webových serverů. Jste možnost tooconfigure, jaká pravidla mají povoleno. Zásady vyjednávání SSL tooconfigure možnost Hello je k dispozici tooallow určité zásady toobe zakázána. Další informace o hello brány firewall webových aplikací, přečtěte si hello toolearn [brány firewall webových aplikací](../application-gateway/application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) článku.

Pokud potřebujete možnost sítě Azure není zadejte, nebo chcete použít místní toouse síťových aplikací, můžete implementovat hello produkty ve virtuálních počítačích a připojte je tooyour virtuální sítě. Hello [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances) obsahuje několik různých virtuálních počítačů, které jsou předem nakonfigurované s síťových aplikací může aktuálně používáte. Tyto předem nakonfigurované virtuální počítače jsou obvykle označují tooas síťových virtuálních zařízení (hodnocení chyb zabezpečení). NVAs jsou k dispozici s aplikací, jako jsou brány firewall a optimalizace sítě WAN.

## <a name="routing"></a>Směrování

Azure vytvoří výchozí směrovací tabulky, které umožňují prostředkům připojených tooany podsíť v žádné virtuální sítě toocommunicate mezi sebou. Můžete implementovat buď nebo obě následující typy trasy toooverride hello hello výchozích tras, které vytvoří Azure:
- **Uživatelem definované:** můžete vytvořit vlastní směrovací tabulky s trasami, které řídí, kde je provoz směrovaný toofor každou podsíť. Další informace o trasy definované uživatelem, přečtěte si hello toolearn [trasy definované uživatelem](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) článku.
- **Border gateway protocol (BGP):** Pokud připojíte vaší virtuální sítě tooyour místní sítí pomocí připojení k Azure VPN Gateway nebo ExpressRoute, můžete rozšířit tooyour trasy protokolu BGP virtuální sítě. BGP je standardní směrovací protokol hello běžně používaný v hello Internet tooexchange směrování a dostupnosti informací mezi dvěma nebo více sítěmi. Když se používá v kontextu hello virtuálních sítí Azure, protokol BGP umožňuje hello Azure VPN Gateway a vaše místní zařízení VPN, partnerský vztah volané protokolu BGP nebo Sousedé BGP, tooexchange "tras", informujte obě brány o hello dostupnosti a dosažitelnosti předpon toogo prostřednictvím hello bránami nebo trasami související se situací. Protokol BGP můžete také povolit směrování přenosu mezi více sítěmi pomocí šíření tras, které brána s protokolem BGP zjistí od jednoho tooall partnera BGP ostatním partnerům BGP. toolearn Další informace o protokolu BGP, najdete v části hello [protokolu BGP se službou Azure VPN Gateway přehled](../vpn-gateway/vpn-gateway-bgp-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) článku.

## <a name="manageability"></a>Možnosti správy

Azure poskytuje následující hello nástroje toomonitor a Správa sítě:
- **Protokoly aktivity:** Azure všechny prostředky mít protokoly aktivity, které poskytují informace o operace, umístěte, stav operací a kdo inicioval hello operaci. Další informace o protokoly aktivity, přečtěte si hello toolearn [aktivity protokoly přehled](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md?toc=%2fazure%2fnetworking%2ftoc.json) článku.
- **Diagnostické protokoly:** periodický a spontánních události jsou vytvořené síťové prostředky a protokolovány v účtech úložiště Azure, odeslané tooan centra událostí Azure nebo odesílání tooAzure analýzy protokolů. Diagnostické protokoly poskytují přehled toohello stavu prostředku. Diagnostické protokoly jsou uvedené pro nástroj pro vyrovnávání zatížení (internetový), skupiny zabezpečení sítě, trasy a aplikační brány. Další informace o diagnostických protokolů, přečtěte si hello toolearn [diagnostické protokoly přehled](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md?toc=%2fazure%2fnetworking%2ftoc.json) článku.
- **Metrika:** jsou metriky měření výkonu a čítače, které jsou shromažďovány prostřednictvím v časovém intervalu na prostředky. Metriky lze použít tootrigger výstrahy na základě prahových hodnot. Aktuálně jsou k dispozici na aplikační brána metriky. Další informace o metriky, přečtěte si hello toolearn [metriky přehled](../monitoring-and-diagnostics/monitoring-overview-metrics.md?toc=%2fazure%2fnetworking%2ftoc.json) článku.
- **Řešení potíží:** informace o odstraňování potíží je přístupná přímo ve hello portálu Azure. informace o Hello pomáhá diagnostikovat běžné problémy s ExpressRoute, brána sítě VPN, aplikační bránu, protokoly zabezpečení sítě, trasy, DNS, nástroj pro vyrovnávání zatížení a Traffic Manager.
- **Řízení přístupu na základě role (RBAC):** určovat, kdo může vytvářet a spravovat síťových prostředků pomocí řízení přístupu na základě rolí (RBAC). Další informace o RBAC načtením hello [začít pracovat s RBAC](../active-directory/role-based-access-control-what-is.md?toc=%2fazure%2fnetworking%2ftoc.json) článku. 
- **Zachytáváním paketů:** hello sledovací proces sítě Azure, které nabízí služba hello možnost toorun paket zachycení virtuálního počítače prostřednictvím rozšíření v rámci hello virtuálních počítačů. Tato možnost je k dispozici pro systémy Linux a virtuální počítače Windows. Další informace o zachytáváním paketů, přečtěte si hello toolearn [přehled zachytávání paketů](../network-watcher/network-watcher-packet-capture-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) článku.
- **Ověřte IP toky:** sledovací proces sítě vám umožní tooverify IP toků mezi virtuální počítač Azure a toodetermine vzdálený prostředek zda pakety jsou povolené nebo odepřené. Tato funkce poskytuje správcům možnost hello tooquickly diagnostikovat problémy s připojením. toolearn Další informace o tok tooverify IP, přečtěte si hello [IP tok ověření přehled](../network-watcher/network-watcher-ip-flow-verify-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) článku.
- **Řešení potíží s připojení VPN:** hello VPN funkce Poradce při potížích s sledovací proces sítě poskytuje hello tooquery možnost připojení nebo brány a ověřte stav hello hello prostředků. Další informace o řešení potíží s připojení VPN, přečtěte si hello toolearn [připojení VPN k řešení potíží s přehled](../network-watcher/network-watcher-troubleshoot-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) článku.
- **Zobrazení topologie sítě:** zobrazení grafické reprezentace hello síťovým prostředkům ve virtuální síti s sledovací proces sítě. Další informace o zobrazení síťovou topologii, přečtěte si hello toolearn [přehled topologie](../network-watcher/network-watcher-topology-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) článku.

## <a name="tools"></a>Nástroje pro nasazení a konfigurace

Můžete nasadit a nakonfigurovat Azure síťových prostředků s žádným z hello následující nástroje:

- **Portál Azure:** grafické uživatelské rozhraní, které běží v prohlížeči. Otevřete hello [portál Azure](http://portal.azure.com).
- **Azure PowerShell:** příkazového řádku nástroje pro správu Azure z počítače se systémem Windows. Další informace o prostředí Azure PowerShell ve čtení hello [Přehled prostředí Azure PowerShell](/powershell/azure/overview?view=azurermps-3.8.0?toc=%2fazure%2fnetworking%2ftoc.json) článku.
- **Rozhraní příkazového řádku Azure (CLI):** příkazového řádku nástroje pro správu Azure z počítačů systému Linux, systému macOS nebo systému Windows. Další informace o hello rozhraní příkazového řádku Azure ve čtení hello [přehled Azure CLI](/cli/azure/get-started-with-azure-cli?toc=%2fazure%2fnetworking%2ftoc.json) článku.
- **Šablony Azure Resource Manageru:** soubor (ve formátu JSON), který definuje hello infrastruktury a konfigurace Azure řešení. Pomocí šablony můžete řešení opakovaně nasadit v průběhu životního cyklu a mít přitom jistotu, že se prostředky nasadí konzistentně. Další informace o vytváření šablon, přečtěte si hello toolearn [osvědčené postupy pro vytváření šablon](../azure-resource-manager/resource-manager-template-best-practices.md?toc=%2fazure%2fnetworking%2ftoc.json) článku. Šablony se dají nasadit s hello portál Azure, rozhraní CLI nebo Powershellu. tooget začít s šablony hned, nasaďte jednu z hello mnoho předem nakonfigurované šablony v hello [šablon Azure rychlý Start](https://azure.microsoft.com/resources/templates/?term=network) knihovny. 

## <a name="pricing"></a>Ceny

Některé z hello Azure mají síťové služby poplatků, zatímco jiné jsou volné. Zobrazení hello [virtuální síť](https://azure.microsoft.com/pricing/details/virtual-network), [brány VPN](https://azure.microsoft.com/pricing/details/vpn-gateway), [Application Gateway](https://azure.microsoft.com/en-us/pricing/details/application-gateway/), [nástroj pro vyrovnávání zatížení](https://azure.microsoft.com/pricing/details/load-balancer), [sledovací proces sítě](https://azure.microsoft.com/pricing/details/network-watcher), [DNS](https://azure.microsoft.com/pricing/details/dns), [Traffic Manager](https://azure.microsoft.com/pricing/details/traffic-manager) a [ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute) ceny stránky pro další informace.

## <a name="next-steps"></a>Další kroky

- Vytvoření vaší první virtuální sítě a připojení tooit několik virtuálních počítačů, pomocí kroků hello v hello [vytvoření vaší první virtuální síť](../virtual-network/virtual-network-get-started-vnet-subnet.md?toc=%2fazure%2fnetworking%2ftoc.json) článku.
- Připojit vaše počítače tooa virtuální sítě pomocí kroků hello v hello [konfigurace připojení typu point-to-site](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) článku.
- Internetové přenosy toopublic servery vyrovnávat zatížení pomocí kroků hello v hello [vytvořit nástroj pro vyrovnávání zatížení internetového](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) článku.
