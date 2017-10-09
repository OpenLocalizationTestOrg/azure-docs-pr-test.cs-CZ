### <a name="is-bgp-supported-on-all-azure-vpn-gateway-skus"></a>Je protokol BGP podporován ve všech SKU služby Azure VPN Gateway?
Ne, protokol BGP je podporován ve službách Azure VPN Gateway **Standard** a **HighPerformance**. Pro SKU **Basic** NENÍ podporován.

### <a name="can-i-use-bgp-with-azure-policy-based-vpn-gateways"></a>Je možné protokol BGP používat se službami Azure VPN Gateway se zásadami?
Ne, protokol BGP je podporován pouze u služeb VPN Gateway se směrováním.

### <a name="can-i-use-private-asns-autonomous-system-numbers"></a>Je možné používat privátní čísla ASN (čísla autonomního systému)?
Ano, svá vlastní veřejná čísla ASN nebo privátní čísla ASN můžete používat pro své místní sítě i virtuální sítě Azure.

### <a name="are-there-asns-reserved-by-azure"></a>Existují ASN vyhrazená sítí Azure?
Ano, hello následující čísla ASN jsou vyhrazené Azure pro interní i externí partnerských vztahů:

* Veřejná ASN: 8075, 8076, 12076
* Soukromá ASN: 65515, 65517, 65518, 65519, 65520

Tyto čísla ASN nelze zadat pro vašeho místního zařízení VPN při připojení brány VPN tooAzure.

### <a name="are-there-any-other-asns-that-i-cant-use"></a>Existují nějaká další ASN, která nejde použít?
Ano, jsou následující čísla ASN hello [rezervován IANA](http://www.iana.org/assignments/iana-as-numbers-special-registry/iana-as-numbers-special-registry.xhtml) a nelze jej nakonfigurovat na služby Azure VPN Gateway:

23456, 64496–64511, 65535–65551 a 429496729

### <a name="can-i-use-hello-same-asn-for-both-on-premises-vpn-networks-and-azure-vnets"></a>Můžete použít hello stejné ASN i místní sítě VPN a sítě Azure Vnet?
Ne, pokud pomocí protokolu BGP vzájemně propojujete místní sítě a sítě Azure VNet, je nutné pro ně přiřadit různá čísla ASN. Službám Azure VPN Gateway je přiřazeno výchozí číslo ASN 65515, ať už je protokol BGP povolen pro připojení mezi místními sítěmi, či nikoliv. Můžete přepsat toto výchozí nastavení tak, že přiřazení jiné číslo ASN při vytváření brány VPN hello nebo po vytvoření brány hello změnit hello číslo ASN. Je nutné tooassign vaše místní čísla ASN toohello odpovídající bránám místních sítí Azure.

### <a name="what-address-prefixes-will-azure-vpn-gateways-advertise-toome"></a>Jakou adresu předpony bude Azure VPN Gateway prezentovat toome?
Služba Azure VPN gateway bude inzerovat hello následující trasy tooyour místní BGP zařízení:

* předpony adres sítí VNet
* Předpony adres pro každá brána Azure VPN připojené toohello brány místní sítě
* Směrování převzatá z jiných BGP partnerského vztahu relace připojené toohello Azure VPN gateway, **kromě výchozího směrování nebo směrování překrytých žádné virtuální sítě předponu**.

### <a name="can-i-advertise-default-route-00000-tooazure-vpn-gateways"></a>Můžete výchozí trasa (0.0.0.0/0) tooAzure VPN Gateway prezentovat?
Ano.

Poznámka: Tato akce vynutí všechny odchozí přenosy virtuální sítě směrem k místní lokalitě a zabrání přijetí veřejné komunikace z hello Internetu přímo, takové protokolu RDP nebo SSH z Internetu toohello hello virtuální počítače hello virtuální sítě virtuálních počítačů.

### <a name="can-i-advertise-hello-exact-prefixes-as-my-virtual-network-prefixes"></a>Můžete přesnou předpony hello inzerovat jako předpony Můj virtuální sítě?

Ne, hello reklamy, stejně jako kterákoli předpon adres virtuální sítě předpony se zablokují nebo filtrovaná podle hello platformy Azure. Můžete ale inzerovat předponu, která je nadmnožinou toho, co máte ve své virtuální síti. 

Například pokud virtuální sítě používá hello adres místo 10.0.0.0/16, může inzerovat 10.0.0.0/8. Nemůžete ale inzerovat 10.0.0.0/16 nebo 10.0.0.0/24.

### <a name="can-i-use-bgp-with-my-vnet-to-vnet-connections"></a>Je možné použít protokol BGP u připojení mezi sítěmi VNet?
Ano, protokol BGP lze použít pro připojení mezi místními sítěmi i připojení mezi sítěmi VNet.

### <a name="can-i-mix-bgp-with-non-bgp-connections-for-my-azure-vpn-gateways"></a>Lze u služeb Azure VPN Gateway používat kombinaci připojení pomocí protokolu BGP a bez protokolu BGP?
Ano, je možné kombinovat i protokolu BGP a bez protokolu BGP připojení pro hello tutéž bránu Azure VPN.

### <a name="does-azure-vpn-gateway-support-bgp-transit-routing"></a>Podporuje služba Azure VPN Gateway směrování provozu pomocí protokolu BGP?
Ano, směrování provozu pomocí protokolu BGP je podporováno, s výjimkou hello, který bude Azure VPN Gateway **není** inzerování výchozích tras tooother partnerské uzly protokolu BGP. tooenable přenosu směrování mezi více bran Azure VPN, musíte povolit protokol BGP na všechny zprostředkující připojení VNet-to-VNet.

### <a name="can-i-have-more-than-one-tunnel-between-azure-vpn-gateway-and-my-on-premises-network"></a>Je možné mít víc než jeden tunel mezi službou Azure VPN Gateway a místní sítí?
Ano, můžete vytvořit více než jeden tunel VPN S2S mezi službou Azure VPN Gateway a místní sítí. Upozorňujeme, že všechny tyto tunely započítají vůči hello celkový počet tunelových propojení pro Azure VPN Gateway a je nutné povolit protokol BGP na obou tunely.

Například pokud máte dva redundantní tunely mezi službou Azure VPN gateway a jednou z vaší místní sítě, budou spotřebovávat 2 tunelů mimo hello celkové kvóty pro bránu Azure VPN (10 pro Standard) a služba HighPerformance 30 tunelů.

### <a name="can-i-have-multiple-tunnels-between-two-azure-vnets-with-bgp"></a>Mohu mít více různých tunelů mezi dvěma sítěmi Azure VNet s protokolem BGP?
Ano, ale alespoň jeden z brány virtuální sítě hello musí být v konfiguraci aktivní aktivní.

### <a name="can-i-use-bgp-for-s2s-vpn-in-an-expressroutes2s-vpn-co-existence-configuration"></a>Je možné použít protokol BGP pro síť VPN S2S provozovanou v koexistenci se sítí VPN ExpressRoute?
Ano. 

### <a name="what-address-does-azure-vpn-gateway-use-for-bgp-peer-ip"></a>Jakou adresu služba Azure VPN Gateway používá pro IP adresu partnera BGP?
Hello Azure VPN gateway přidělí jednu IP adresu z rozsahu GatewaySubnet definovaného pro virtuální síť hello hello. Ve výchozím nastavení je hello předposlední adresu hello rozsahu. Například pokud GatewaySubnet 10.12.255.0/27, od 10.12.255.0 too10.12.255.31 hello IP adresy partnera BGP na hello Azure VPN gateway bude 10.12.255.30. Tyto informace můžete najít zobrazením hello informace o bráně Azure VPN.

### <a name="what-are-hello-requirements-for-hello-bgp-peer-ip-addresses-on-my-vpn-device"></a>Jaké jsou požadavky hello hello IP adresu partnera BGP v zařízení VPN?
Vaše adresa partnera BGP místní **nesmí** být hello stejné jako hello veřejnou IP adresu vašeho zařízení VPN. Použijte jinou IP adresu pro vaše IP adresa partnera BGP v zařízení VPN hello. Může být o adresu přiřazenou rozhraní zpětné smyčky toohello na hello zařízení. Adresu zadejte v hello odpovídající bráně místní sítě představující hello umístění.

### <a name="what-should-i-specify-as-my-address-prefixes-for-hello-local-network-gateway-when-i-use-bgp"></a>Co je třeba zadat jako předpony adres pro hello bránu místní sítě při použití protokolu BGP?
Azure Local Network Gateway určuje hello počáteční předpony adres pro hello do místní sítě. Pomocí protokolu BGP, je třeba přiřadit hello předpona hostitele (předponu / 32) vaší adresy IP adresa partnera BGP jako adresní prostor hello pro danou místní síť. Pokud je vaše IP adresa partnera BGP 10.52.255.254, musíte zadat "10.52.255.254/32" jako hodnotu localNetworkAddressSpace hello hello místní síťové brány reprezentující tento místní sítě. Toto je tooensure, který hello Azure VPN gateway vytvoří relaci BGP hello prostřednictvím tunelu S2S VPN hello.

### <a name="what-should-i-add-toomy-on-premises-vpn-device-for-hello-bgp-peering-session"></a>Co měli přidat zařízení toomy místní VPN pro relaci partnerského vztahu protokolu BGP hello?
Měli byste přidat směrování hostitele hello Azure IP adresy partnera BGP v zařízení VPN odkazující na tunel IPsec S2S VPN toohello. Například pokud hello IP adresa partnera BGP Azure VPN "10.12.255.30", je nutné přidat směrování hostitele pro "10.12.255.30" s rozhraním nexthop odpovídajícího rozhraní tunelu IPsec hello ve vašem zařízení VPN.

