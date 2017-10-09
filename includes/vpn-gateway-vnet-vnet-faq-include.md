Nejčastější dotazy týkající se propojení VNet-to-VNet Hello platí tooVPN připojení brány. Pokud hledáte informace o VNet Peering, přečtěte si téma [Partnerské vztahy virtuálních sítí](../articles/virtual-network/virtual-network-peering-overview.md).

### <a name="does-azure-charge-for-traffic-between-vnets"></a>Účtuje se v Azure provoz mezi virtuálními sítěmi?

Provoz VNet-to-VNet v rámci hello stejné oblasti je obou směrech zdarma při použití připojení k bráně VPN. Pro různé oblasti VNet-to-VNet odchozí provoz je pověřen hello odchozí mezi virtuálními sazby za přenos dat podle zdrojových oblastí hello. Odkazovat toohello [brány VPN stránce s cenami](https://azure.microsoft.com/pricing/details/vpn-gateway/) podrobnosti. Pokud se připojujete vaší virtuální sítě pomocí virtuální sítě partnerský vztah, nikoli brány sítě VPN, přečtěte si téma hello [virtuální sítě stránce s cenami](https://azure.microsoft.com/pricing/details/virtual-network/).

### <a name="does-vnet-to-vnet-traffic-travel-across-hello-internet"></a>Cestují provoz VNet-to-VNet přes hello Internetu?

Ne. Provoz VNet-to-VNet se přenáší po hello páteřní Microsoft Azure, hello Internetu.

### <a name="is-vnet-to-vnet-traffic-secure"></a>Je provoz VNet-to-VNet bezpečný?

Ano, je chráněn šifrováním s použitím protokolu IPsec/IKE.

### <a name="do-i-need-a-vpn-device-tooconnect-vnets-together"></a>Společně tooconnect zařízení VPN virtuální sítě potřeba?

Ne. Propojení více virtuálních sítí Azure nevyžaduje žádná zařízení VPN, pokud není vyžadována možnost připojení mezi různými místy.

### <a name="do-my-vnets-need-toobe-in-hello-same-region"></a>Můj virtuální sítě nutná toobe v hello stejné oblasti?

Ne. Hello virtuální sítě může být v hello oblastí Azure stejný nebo jiný (umístění).

### <a name="if-hello-vnets-are-not-in-hello-same-subscription-do-hello-subscriptions-need-toobe-associated-with-hello-same-ad-tenant"></a>Pokud hello virtuálních sítí nejsou v hello stejné předplatné, odběry hello potřebují toobe přidružený klientovi hello stejnou AD?

Ne.

### <a name="can-i-use-vnet-to-vnet-along-with-multi-site-connections"></a>Je možné použít VNet-to-VNet společně s připojením propojujícím víc serverů?

Ano. Možnost připojení k virtuální síti je možné využívat současně se sítěmi VPN s více servery.

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a>Ke kolika místních serverům a virtuálním sítím se může připojit jedna virtuální síť?

Viz tabulka [Požadavky na bránu](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#requirements).

### <a name="can-i-use-vnet-to-vnet-tooconnect-vms-or-cloud-services-outside-of-a-vnet"></a>Můžete použít VNet-to-VNet tooconnect virtuálních počítačů nebo cloudových služeb mimo virtuální síť?

Ne. Propojení VNet-to-VNet podporují propojování virtuálních sítí. Nepodporuje propojování virtuálních počítačů ani cloudových služeb mimo virtuální síť.

### <a name="can-a-cloud-service-or-a-load-balancing-endpoint-span-vnets"></a>Může cloudová služba nebo koncový bod vyrovnávání zatížení pracovat nad více virtuálními sítěmi?

Ne. Cloudová služba ani koncový bod vyrovnávání zatížení nemůžou pracovat nad více virtuálními sítěmi ani v případě, že jsou propojeny.

### <a name="can-i-used-a-policybased-vpn-type-for-vnet-to-vnet-or-multi-site-connections"></a>Je možné použít typ sítě VPN PolicyBased pro připojení VNet-to-VNet nebo Multi-Site?

Ne. Připojení VNet-to-VNet a Multi-Site vyžadují brány VPN Azure s typy sítě VPN RouteBased (dříve nazývané dynamické směrování).

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-tooanother-vnet-with-a-policybased-vpn-type"></a>Je možné připojit virtuální síť s typ sítě VPN RouteBased tooanother virtuální síť s typu Policybased?

Ne, obě virtuální sítě MUSÍ používat sítě VPN založené na směrování (dříve nazývané dynamické směrování).

### <a name="do-vpn-tunnels-share-bandwidth"></a>Sdílejí tunely VPN šířku pásma?

Ano. Všechna tunelová propojení VPN virtuální sítě hello sdílet hello dostupnou šířku pásma hello Azure VPN gateway a hello stejné SLA provozu v Azure VPN gateway.

### <a name="are-redundant-tunnels-supported"></a>Jsou podporovány redundantní tunely?

Redundantní tunely mezi párem virtuálních sítí jsou podporovány, pokud je jedna brána virtuální sítě nakonfigurována jako aktivní-aktivní.

### <a name="can-i-have-overlapping-address-spaces-for-vnet-to-vnet-configurations"></a>Můžou se překrývat adresní prostory pro konfigurace VNet-to-VNet?

Ne. Není možné, aby se rozsahy IP adres překrývaly.

### <a name="can-there-be-overlapping-address-spaces-among-connected-virtual-networks-and-on-premises-local-sites"></a>Můžou se překrývat adresní prostory mezi propojenými virtuálními sítěmi a místními servery?

Ne. Není možné, aby se rozsahy IP adres překrývaly.



