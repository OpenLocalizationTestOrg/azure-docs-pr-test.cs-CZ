Hello následující tabulka uvádí typy hello brány a odhadovanou agregovanou propustnost hello podle skladová položka brány. Tato tabulka platí toohello Resource Manager a modely nasazení classic. 

Ceny se liší pro jednotlivé SKU brány. Další informace najdete v tématu [VPN Gateway – ceny](https://azure.microsoft.com/pricing/details/vpn-gateway).

Všimněte si, že hello UltraPerformance brány, není v této tabulce reprezentována SKU. Informace o hello UltraPerformance SKU najdete v tématu hello [ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md) dokumentaci.

|  | **Propustnost brány sítě VPN (1)** | **Maximální počet tunelových propojení IPsec brány sítě VPN (2)** | **Propustnost brány ExpressRoute** | **Brána sítě VPN a ExpressRoute vedle sebe** |
| --- | --- | --- | --- | --- |
| **Základní SKU (3)(5)(6)** |100 Mb/s |10 |500 Mb/s (6) |Ne |
| **Standardní SKU (4)(5)** |100 Mb/s |10 |1000 Mb/s |Ano |
| **SKU pro vysoký výkon (4)** |200 Mb/s |30 |2000 Mb/s |Ano |


(1) propustnost sítě VPN hello je odhad na základě měření hello mezi virtuálními sítěmi v hello stejné oblasti Azure. Není zaručenou propustnost pro připojení mezi různými místy napříč hello Internetu. Je měření maximální možné propustnost hello.

(2) hello počet tunelových propojení naleznete tooRouteBased sítě VPN. Sítě VPN PolicyBased můžou podporovat jen jeden tunel VPN typu S2S (Site-to-Site).

(3) protokol BGP není podporován pro hello základní SKU.

(4) Sítě VPN typu PolicyBased nejsou pro tuto SKU podporované. Pro hello základní SKU pouze jsou podporovány.

(5) Propojení VPN Gateway S2S aktivní-aktivní nejsou pro toto SKU podporovaná. Aktivní aktivní je podporována v pouze hello HighPerformance SKU.

(6) základní SKU je zastaralý pro použití se službou ExpressRoute.
