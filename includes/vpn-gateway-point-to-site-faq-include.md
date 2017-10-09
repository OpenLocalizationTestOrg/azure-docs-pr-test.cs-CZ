### <a name="what-client-operating-systems-can-i-use-with-point-to-site"></a>Jaké klientské operační systémy je možné používat s připojeními typu Point-to-Site?

podporovány jsou následující operační systémy klienta Hello:

* Windows 7 (32bitové a 64bitové verze)
* Windows Server 2008 R2 (pouze 64bitové verze)
* Windows 8 (32bitové a 64bitové verze)
* Windows 8.1 (32bitové a 64bitové verze)
* Windows Server 2012 (pouze 64bitové verze)
* Windows Server 2012 R2 (pouze 64bitové verze)
* Windows 10

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp"></a>Je možné použít libovolného softwarového klienta sítě VPN pro připojení Point-to-Site, pokud podporuje protokol SSTP?

Ne. Podpora je omezena pouze toohello verze operačních systémů Windows uvedené výše.

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Kolik koncových bodů klienta VPN je možné mít v konfiguraci připojení Point-to-Site?

Podporujeme až too128 klienti toobe možné tooconnect tooa virtuální síť VPN na hello stejný čas.

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Je možné používat vlastní interní kořenové certifikační autority infrastruktury veřejných klíčů pro připojení Point-to-Site?

Ano. Dříve bylo možné používat pouze kořenové certifikáty podepsané svým držitelem. I nyní je možné nahrát 20 kořenových certifikátů.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Je možné procházet proxy servery a brány firewall s využitím schopnosti Point-to-Site?

Ano. Používáme tootunnel SSTP (Secure Socket Tunneling Protocol) přes brány firewall. Toto tunelové propojení se jeví jako připojení s protokolem HTTPs.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-hello-vpn-automatically-reconnect"></a>Pokud restartuji klientský počítač nakonfigurovaný pro Point-to-Site, bude hello VPN automaticky znovu připojit?

Ve výchozím nastavení hello klientský počítač nebude znovu vytvořit připojení k síti VPN hello automaticky.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-hello-vpn-clients"></a>Nepodporuje podporu Point-to-Site automatické opětné připojení a DDNS u hello klientům VPN?

Automatické opětné připojení a DDNS se u sítí VPN s připojením Point-to-Site aktuálně nepodporují.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-hello-same-virtual-network"></a>Může mít Site-to-Site a Point-to-Site konfigurace existovat vedle sebe hello stejné virtuální sítě?

Ano. Obě tato řešení budou funkční, pokud pro bránu používáte síť VPN typu RouteBased. Pro hello modelu nasazení classic budete potřebovat dynamickou bránu. Provedeme není podpora Point-to-Site pro brány VPN se statickým směrováním ani brány používající hello `-VpnType PolicyBased` rutiny.

### <a name="can-i-configure-a-point-to-site-client-tooconnect-toomultiple-virtual-networks-at-hello-same-time"></a>Můžete konfigurovat Point-to-Site klienta tooconnect toomultiple virtuální sítě na hello současně?

Ano, je to možné. Ale hello virtuální sítě nemůže mít překrývající se předpony IP a hello Point-to-Site adresní prostory se nesmí překrývat mezi virtuálními sítěmi hello.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Jakou propustnost je možné očekávat u připojení typu Site-to-Site nebo Point-to-Site?

Je obtížné toomaintain hello přesnou propustnost tunelových propojení VPN hello. IPsec a SSTP jsou kryptograficky náročné protokoly sítě VPN. Propustnost je také omezena latencí hello a šířky pásma mezi místy a hello Internetu.
