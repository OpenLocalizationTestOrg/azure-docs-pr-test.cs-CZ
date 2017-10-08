---
title: "aaaAzure VPN Gateway – nejčastější dotazy | Microsoft Docs"
description: "Hello VPN Gateway – nejčastější dotazy. Nejčastější dotazy týkající se propojení Microsoft Azure Virtual Network mezi různými místy, připojení s hybridní konfigurací a bran VPN"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
ms.assetid: 6ce36765-250e-444b-bfc7-5f9ec7ce0742
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/30/2017
ms.author: cherylmc,yushwang
ms.openlocfilehash: e1737f5832728f513e31f97cc7e752147faaaeb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="vpn-gateway-faq"></a>Nejčastější dotazy k branám VPN

## <a name="connecting"></a>Propojení sítí toovirtual

### <a name="can-i-connect-virtual-networks-in-different-azure-regions"></a>Je možné propojit virtuální sítě v různých oblastech Azure?

Ano. Žádné omezení oblastí se ve skutečnosti neuplatňuje. Jedna virtuální síť můžete připojit virtuální síť tooanother v hello stejné oblasti, nebo v jiné oblasti Azure. 

### <a name="can-i-connect-virtual-networks-in-different-subscriptions"></a>Je možné propojovat virtuální sítě v rámci různých předplatných?

Ano.

### <a name="can-i-connect-toomultiple-sites-from-a-single-virtual-network"></a>Můžete připojit toomultiple lokality z jedné virtuální sítě?

Toomultiple lokalit můžete připojit pomocí prostředí Windows PowerShell a hello rozhraní API REST služby Azure. V tématu hello [k více serverům a připojení VNet-to-VNet](#V2VMulti) v části Nejčastější dotazy.

### <a name="what-are-my-cross-premises-connection-options"></a>Jaké jsou možnosti připojení mezi různými místy?

Hello následující mezi různými místy, že připojení jsou podporovány:

* Site-to-Site – připojení VPN prostřednictvím protokolu IPsec (IKE v1 a IKE v2). Tento typ připojení vyžaduje zařízení VPN nebo službu RRAS. Další informace naleznete v tématu [Site-to-Site](vpn-gateway-howto-site-to-site-resource-manager-portal.md).
* Point-to-Site – připojení VPN prostřednictvím protokolu SSTP (Secure Socket Tunneling Protocol). Toto připojení nevyžaduje zařízení VPN. Další informace naleznete v tématu [Point-to-Site](vpn-gateway-howto-point-to-site-resource-manager-portal.md).
* Propojení VNet-to-VNet – tento typ připojení je hello stejný jako konfigurace Site-to-Site. VNet tooVNet je připojení VPN prostřednictvím protokolu IPsec (IKE v1 a IKE v2). Nevyžaduje zařízení VPN. Další informace naleznete v tématu [VNet-to-VNet](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).
* Více servery – to je varianta konfigurace Site-to-Site, která umožňuje tooconnect více místními lokalitami tooa virtuální sítě. Další informace najdete v tématu [Multi-Site](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).
* ExpressRoute – ExpressRoute je přímé připojení tooAzure z vaší sítě WAN, nikoli připojení VPN prostřednictvím hello veřejného Internetu. Další informace najdete v tématu hello [technický přehled ExpressRoute](../expressroute/expressroute-introduction.md) a hello [ExpressRoute – nejčastější dotazy](../expressroute/expressroute-faqs.md).

Další informace o připojeních VPN Gateway najdete v tématu [Informace o službě VPN Gateway](vpn-gateway-about-vpngateways.md).

### <a name="what-is-hello-difference-between-a-site-to-site-connection-and-point-to-site"></a>Co je hello rozdíl mezi připojení Site-to-Site a Point-to-Site?

Konfigurace **Site-to-Site** (tunel VPN IPsec/IKE) jsou mezi místním umístěním a Azure. To znamená, zda se můžete připojit z libovolného počítače nachází na místním tooany virtuálním počítači nebo instanci role v rámci virtuální sítě, v závislosti na tom, jak provedete tooconfigure směrování a oprávnění. Představuje skvělou možnost připojení mezi různými místy, která je vždy k dispozici a je velmi vhodná pro hybridní konfigurace. Tento typ připojení využívá zařízení sítě VPN IPsec (hardwarové zařízení nebo softwarové zařízení), které musí být nasazeno hello hraniční sítě. toocreate tento typ připojení, musí mít zvenčí IPv4 adresu, která není za adres (NAT)

**Point-to-Site** konfigurace (prostřednictvím protokolu SSTP VPN) umožňují připojit se z jednoho počítače odkudkoli tooanything nachází ve virtuální síti. Používá klienta VPN ve Windows hello. Jako součást konfigurace hello Point-to-Site nainstalujete certifikát a balíček konfigurace klienta VPN, který obsahuje hello nastavení, které umožňují počítače tooconnect tooany virtuálního počítače nebo instance role v rámci virtuální sítě hello. Ho je skvělé, pokud chcete tooconnect tooa virtuální sítě, ale nejsou nacházejí na místních. Je také vhodný Pokud nemáte přístup tooVPN hardwaru nebo zvenčí adresu IPv4, které jsou požadovány pro připojení Site-to-Site.

Toouse vaší virtuální sítě můžete nakonfigurovat i Site-to-Site a Point-to-Site současně, pokud při vytváření připojení Site-to-Site pomocí sítě VPN založené na trasách typu pro bránu. Typy sítě VPN založené na trasách se nazývají dynamické brány v modelu nasazení classic hello.

## <a name="gateways"></a>Brány virtuálních sítí

### <a name="is-a-vpn-gateway-a-virtual-network-gateway"></a>Je brána sítě VPN bránou virtuální sítě?

Brána sítě VPN je typem brány virtuální sítě. Brána sítě VPN odesílá šifrovaný provoz mezi virtuální sítí a místním umístěním přes veřejné připojení. Můžete také použít přenosem toosend brány VPN mezi virtuálními sítěmi. Při vytváření brány VPN použijete hello - GatewayType hodnotu 'Vpn'. Další informace najdete v tématu [Informace o nastavení konfigurace služby VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md).

### <a name="what-is-a-policy-based-static-routing-gateway"></a>Co je to brána založená na zásadách (se statickým směrováním)?

Brány založené na zásadách implementují sítě VPN založené na zásadách. Sítě VPN založené na zásadách šifrují pakety a směrují je do tunelových propojení IPsec založené na kombinaci hello předpon adres mezi vaší místní sítí a hello virtuální síť Azure. zásada Hello (nebo selektor provozu) je obvykle definováno jako přístupový seznam v konfiguraci sítě VPN hello.

### <a name="what-is-a-route-based-dynamic-routing-gateway"></a>Co je to brána založená na směrování (s dynamickým směrováním)?

Implementace založená na trasách brány hello sítě VPN založené na směrování. Sítě VPN založené na směrování používají hello IP předávání nebo směrovací tabulce toodirect paketů do svých příslušných rozhraní tunelových propojení "trasy". Hello rozhraní tunelového propojení potom zašifrovat nebo dešifrovat hello pakety směřující tunely hello. Hello zásady nebo selektor provozu pro založené na trasách trasách mají konfiguraci typu any-to-any (nebo zástupné znaky).

### <a name="do-i-need-a-gatewaysubnet"></a>Potřebuji GatewaySubnet?

Ano. podsíť brány Hello obsahuje hello IP adresy, které používají služby brány virtuální sítě hello. Potřebujete toocreate podsíť brány pro vaši virtuální síť v pořadí tooconfigure bránu virtuální sítě. Všechny podsítě brány musí mít název "GatewaySubnet" toowork správně. Nenastavujte pro podsíť brány jiný název. A nenasazujte virtuální počítače ani cokoli jiného toohello podsíť brány.

Při vytváření podsítě brány hello určíte, že obsahuje hello počet IP adres, které hello podsítě. Služba brány toohello mají při přidělování Hello IP adresy v podsíti brány hello. Některé konfigurace vyžadují další toobe IP adresy přidělené toohello služby brány než jiné. Chcete toomake se, že podsítě brány obsahuje dostatek IP adresy tooaccommodate budoucímu růstu a možné další nové konfigurace připojení. Přestože je tedy možné vytvořit tak malou podsíť brány, jako je /29, doporučujeme vytvořit podsíť brány o velikosti /27 nebo větší (/27, /26, /25 atd.). Podívejte se na hello požadavky pro konfiguraci hello má toocreate a ověřte, že tuto podsíť brány hello, měli byste bude splňovat tyto požadavky.

### <a name="can-i-deploy-virtual-machines-or-role-instances-toomy-gateway-subnet"></a>Můžete nasadit virtuální počítače nebo podsíť brány toomy instance role?

Ne.

### <a name="can-i-get-my-vpn-gateway-ip-address-before-i-create-it"></a>Je možné získat IP adresu brány VPN předtím, než se vytvoří?

Ne. Máte toocreate vaše první tooget hello IP adresu brány. Pokud odstranit a znovu vytvořte bránu VPN změní Hello IP adresa.

### <a name="can-i-request-a-static-public-ip-address-for-my-vpn-gateway"></a>Je možné vyžádat si pro bránu VPN statickou veřejnou IP adresu?

Ne. Podporuje se pouze dynamické přiřazení IP adresy. Však neznamená to, že hello IP adresa změní po byl přiřazen tooyour brány VPN. Hello pouze čas změny IP adresy hello VPN gateway je při hello brány je odstraní a znovu vytvoří. mezi změny velikosti, resetování nebo jiné operace údržby/upgradu vaší brány VPN nezmění Hello veřejná IP adresa brány VPN. 

### <a name="how-does-my-vpn-tunnel-get-authenticated"></a>Jak se tunelové připojení sítě VPN ověřuje?

Síť VPN Azure používá ověřování PSK (předsdílený klíč). Při vytváření tunelového připojení sítě VPN hello se vygeneruje předsdílený klíč (PSK). Hello automaticky generovaný PSK tooyour vlastní hello nastavení předsdíleného klíče rutiny prostředí PowerShell nebo rozhraní REST API, můžete změnit.

### <a name="can-i-use-hello-set-pre-shared-key-api-tooconfigure-my-policy-based-static-routing-gateway-vpn"></a>Je možné používat hello nastavení předsdíleného klíče API tooconfigure založenou na zásadách (se statickým směrováním) brány sítě VPN?

Ano, hello nastavení předsdíleného klíče API i rutinu prostředí PowerShell může být použité tooconfigure Azure založené na zásadách (se statickým směrováním) i na základě trasy (dynamické) směrování sítě VPN.

### <a name="can-i-use-other-authentication-options"></a>Je možné použít jiné možnosti ověřování?

Snažíme se omezené toousing předsdílených klíčů (PSK) pro ověřování.

### <a name="how-do-i-specify-which-traffic-goes-through-hello-vpn-gateway"></a>Jak který zadejte provoz prochází hello brána sítě VPN?

#### <a name="resource-manager-deployment-model"></a>Model nasazení Resource Manager

* Prostředí PowerShell: použijte "AddressPrefix" toospecify provoz pro bránu místní sítě hello.
* Portál Azure: přejděte brány místní sítě toohello > Konfigurace > adresní prostor.

#### <a name="classic-deployment-model"></a>Model nasazení Classic

* Portál Azure: přejděte toohello klasickou virtuální síť > připojení k síti VPN > připojení Site-to-site VPN > název místního webu > Místní lokality > klienta adresní prostor. 
* Portál Classic: přidejte všechny rozsahy, které chcete odeslaných prostřednictvím hello brány pro vaši virtuální síť na stránce hello sítě v části místní sítě. 

### <a name="can-i-configure-forced-tunneling"></a>Je možné konfigurovat vynucené tunelování?

Ano. Informace najdete v části [Konfigurace vynuceného tunelování](vpn-gateway-about-forced-tunneling.md).

### <a name="can-i-set-up-my-own-vpn-server-in-azure-and-use-it-tooconnect-toomy-on-premises-network"></a>Můžete nastavit vlastní server VPN v Azure a použít ho tooconnect toomy do místní sítě?

Ano, můžete nasadit vlastní servery v Azure buď z Azure Marketplace hello nebo vytvořit vlastní směrovače sítě VPN nebo brány VPN. Je třeba trasy definované uživatelem tooconfigure ve virtuální síti tooensure provoz se směruje správně mezi místními sítěmi a podsítěmi virtuálních sítí.

### <a name="why-are-certain-ports-opened-on-my-vpn-gateway"></a>Proč jsou některé porty brány VPN otevřené?

Jsou vyžadovány pro komunikaci infrastruktury Azure. Jsou chráněny (uzamknuty) s použitím certifikátů Azure. Bez správných certifikátů nemohou externí entity, včetně hello zákazníků těchto bran, nebude možné toocause žádný vliv na těchto koncových bodů.

Brána sítě VPN je zásadně vícedomé zařízení s jednou síťovou KARTOU klepnutím na hello privátní síti zákazníka a jeden síťový adaptér přístupných hello veřejné síti. Entity infrastruktury Azure nelze klepněte do privátních sítí zákazníků kvůli dodržování předpisů, které potřebují tooutilize veřejné koncové body pro komunikaci infrastruktury. Hello veřejné koncové body jsou pravidelně kontrolovány auditem zabezpečení Azure.

### <a name="more-information-about-gateway-types-requirements-and-throughput"></a>Další informace o typech brány, požadavcích a propustnosti

Další informace najdete v tématu [Informace o nastavení konfigurace služby VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md).

## <a name="s2s"></a>Připojení typu Site-to-Site a zařízení VPN

### <a name="what-should-i-consider-when-selecting-a-vpn-device"></a>Co je třeba zvážit při výběru zařízení VPN?

Ve spolupráci s dodavateli zařízení jsme ověřili sadu standardních zařízení VPN pro připojení Site-to-Site. Seznam známých kompatibilních zařízení VPN, příslušné pokyny ke konfiguraci nebo ukázky a specifikace zařízení najdete v hello [informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md) článku. Všechna zařízení v řadách zařízení hello uvedená jako známá kompatibilní by měla spolupracovat s virtuální sítí. toohelp konfigurace zařízení VPN, získáte toohello ukázka konfigurace zařízení nebo odkaz, který odpovídá řada tooappropriate zařízení.

### <a name="where-can-i-find-vpn-device-configuration-settings"></a>Kde najdu nastavení konfigurace zařízení VPN?

[!INCLUDE [vpn devices](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

### <a name="how-do-i-edit-vpn-device-configuration-samples"></a>Jak upravím ukázky konfigurace zařízení VPN?

Informace o úpravách ukázek konfigurace zařízení najdete v tématu popisujícím [úpravy ukázek](vpn-gateway-about-vpn-devices.md#editing).

### <a name="where-do-i-find-ipsec-and-ike-parameters"></a>Kde najdu parametry protokolu IPsec a IKE?

Parametry protokolu IPsec/IKE najdete v popisu [parametrů](vpn-gateway-about-vpn-devices.md#ipsec).

### <a name="why-does-my-policy-based-vpn-tunnel-go-down-when-traffic-is-idle"></a>Proč se tunelové připojení sítě VPN založené na zásadách při nečinnosti deaktivuje?

Toto je očekávané chování u bran VPN pracujících na základě zásad (označují se také výrazem statické směrování). Při provozu hello přes tunel hello nečinný déle než 5 minut, tunelové propojení hello se deaktivuje. Při provozu v některém směru obnoví, tunelové propojení hello se znovu vytvoří okamžitě.

### <a name="can-i-use-software-vpns-tooconnect-tooazure"></a>Můžete použít tooAzure tooconnect softwaru sítě VPN?

Konfigurace připojení Site-to-Site pro více míst je podporována u serverů RRAS (Routing and Remote Access) Windows Server 2012.

Další softwarová řešení sítě VPN by měla s naší bránou spolupracovat, dokud jsou v souladu s tooindustry standardní implementacím protokolu IPsec. Obraťte se na dodavatele hello hello softwaru pokyny pro konfiguraci a podporu.

## <a name="P2S"></a>Připojení typu Point-to-Site

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="V2VMulti"></a>Připojení typu VNet-to-VNet a Multi-Site

[!INCLUDE [vpn-gateway-vnet-vnet-faq-include](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

### <a name="can-i-use-azure-vpn-gateway-tootransit-traffic-between-my-on-premises-sites-or-tooanother-virtual-network"></a>Můžete použít Azure VPN gateway tootransit provoz mezi Moje místními servery nebo tooanother virtuální sítě?

**Model nasazení Resource Manager**<br>
Ano. V tématu hello [BGP](#bgp) části Další informace.

**Model nasazení Classic**<br>
Provoz prostřednictvím brány Azure VPN je možné pomocí modelu nasazení classic hello, ale spoléhá na staticky definovaných adresních prostorech v konfiguračním souboru na hello sítě. Protokol BGP není dosud podporována s virtuální sítí Azure a síť VPN Gateway pomocí modelu nasazení classic hello. Bez protokolu BGP je ruční definování adresních prostorů pro přenos velmi náchylné k chybám a nedoporučuje se.

### <a name="does-azure-generate-hello-same-ipsecike-pre-shared-key-for-all-my-vpn-connections-for-hello-same-virtual-network"></a>Generuje Azure stejný protokolu IPsec/IKE předsdíleným hello klíč pro všechna připojení k síti VPN pro hello stejné virtuální sítě?

Ne, Azure ve výchozím nastavení pro různá připojení k síti VPN generuje různé předsdílené klíče. Však můžete hello nastavit síť VPN brány klíč rozhraní API REST nebo PowerShell rutinu tooset hello hodnota klíče, který preferujete. Hello klíč musí být alfanumerický řetězec o délce 1 too128 znaků.

### <a name="do-i-get-more-bandwidth-with-more-site-to-site-vpns-than-for-a-single-virtual-network"></a>Je možné dosáhnout větší šířky pásma použitím několika sítí VPN Site-to-Site než v případě jedné virtuální sítě?

Ne, všechna tunelové propojení sítí VPN Point-to-Site VPN, sdílet hello stejné Azure VPN gateway a hello dostupnou šířku pásma.

### <a name="can-i-configure-multiple-tunnels-between-my-virtual-network-and-my-on-premises-site-using-multi-site-vpn"></a>Je možné nakonfigurovat více tunelových propojení mezi virtuální sítí a místního serverem prostřednictvím sítě VPN pro více serverů?

Ano, ale musíte nakonfigurovat protokol BGP na obou toohello tunely stejné umístění.

### <a name="can-i-use-point-to-site-vpns-with-my-virtual-network-with-multiple-vpn-tunnels"></a>Je možné používat sítě VPN Point-to-Site v případě virtuální sítě s několika tunelovými propojeními sítě VPN?

Ano, sítě VPN Point-to-Site (P2S) lze použít s bránami VPN hello připojení toomultiple místními servery a jiné virtuální sítě.

### <a name="can-i-connect-a-virtual-network-with-ipsec-vpns-toomy-expressroute-circuit"></a>Můžete připojit virtuální síť pomocí protokolu IPsec VPN toomy okruh ExpressRoute?

Ano, tato možnost je podporována. Další informace najdete v tématu [konfigurace ExpressRoute a připojení VPN typu site-to-site, která mohou existovat vedle sebe](../expressroute/expressroute-howto-coexist-classic.md).

## <a name="ipsecike"></a>Zásady IPsec/IKE

[!INCLUDE [vpn-gateway-ipsecikepolicy-faq-include](../../includes/vpn-gateway-ipsecikepolicy-faq-include.md)]


## <a name="bgp"></a>BGP

[!INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)]

## <a name="vms"></a>Připojení mezi místními sítěmi a virtuální počítače

### <a name="if-my-virtual-machine-is-in-a-virtual-network-and-i-have-a-cross-premises-connection-how-should-i-connect-toohello-vm"></a>Pokud virtuální počítač nachází ve virtuální síti a je nutné připojení mezi různými místy, jak připojovat toohello virtuálního počítače?

Možností je několik. Pokud máte protokolu RDP pro virtuální počítač povolena, můžete připojit tooyour virtuálního počítače pomocí hello privátní IP adresu. V takovém případě zadáte hello privátní IP adresu a port hello, které chcete tooconnect příliš (obvykle 3389). Budete potřebovat tooconfigure hello port ve virtuálním počítači pro provoz hello.

Můžete také připojit tooyour virtuálního počítače pomocí privátní IP adresu z jiného virtuálního počítače, který je umístěný na hello stejné virtuální síti. Nemůžete RDP tooyour virtuálního počítače pomocí hello privátní IP adresu, pokud se připojujete z místa mimo virtuální síť. Například pokud máte virtuální síť Point-to-Site nakonfigurovat a z vašeho počítače není vytvořeno připojení, nemůžete se připojit toohello virtuálního počítače pomocí privátní IP adresy.

### <a name="if-my-virtual-machine-is-in-a-virtual-network-with-cross-premises-connectivity-does-all-hello-traffic-from-my-vm-go-through-that-connection"></a>Pokud virtuální počítač nachází ve virtuální síti s připojením mezi různými místy, přejděte veškerý provoz hello z virtuálního počítače přes toto připojení?

Ne. Jenom provoz hello s cílovou IP, která je součástí hello virtuální sítě místní síťové rozsahy IP adres, které jste zadali bude projít hello brány virtuální sítě. Provoz má cíl, který se nachází v rámci virtuální sítě hello IP zůstává v rámci virtuální sítě hello. Ostatní přenosy budou odesílat prostřednictvím hello zatížení vyrovnávání toohello veřejných sítích, nebo pokud je použito vynucené tunelování, budou odesílat prostřednictvím brány Azure VPN hello.

### <a name="how-do-i-troubleshoot-an-rdp-connection-tooa-vm"></a>Jak odstranit tooa připojení RDP virtuálního počítače

[!INCLUDE [Troubleshoot VM connection](../../includes/vpn-gateway-connect-vm-troubleshoot-include.md)]


## <a name="faq"></a>Nejčastější dotazy týkající se virtuálních sítí

Zobrazit další virtuální síťové informace v hello [virtuální sítě – nejčastější dotazy](../virtual-network/virtual-networks-faq.md).

## <a name="next-steps"></a>Další kroky

* Další informace o službě VPN Gateway najdete v tématu [Informace o službě VPN Gateway](vpn-gateway-about-vpngateways.md).
* Další informace o nastavení konfigurace služby VPN Gateway najdete v tématu [Informace o nastavení konfigurace služby VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md).
