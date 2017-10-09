Když vytvoříte bránu virtuální sítě, je nutné toospecify hello brány, které chcete toouse SKU. Vyberte hello SKU, které splňují vaše požadavky na základě typů hello zatížení, propustnost, funkce a SLA.

[!INCLUDE [classic SKU](./vpn-gateway-classic-sku-support-include.md)]

[!INCLUDE [Aggregated throughput by SKU](./vpn-gateway-table-gwtype-aggtput-include.md)]

###  <a name="workloads"></a>Produkční *vs.* vývojářské a testovací úlohy

Z důvodu toohello rozdíly v SLA a sady funkcí, doporučujeme následující SKU pro produkční prostředí hello *oproti* vývoj testování:

| **Úloha**                       | **SKU**               |
| ---                                | ---                    |
| **Produkce, kritické úlohy** | VpnGw1, VpnGw2, VpnGw3 |
| **Vývoj a testování nebo testování konceptu**   | Basic                  |
|                                    |                        |

Pokud používáte hello staré SKU, hello produkční SKU doporučení jsou standardní a HighPerformance SKU. Informace o hello staré SKU, najdete v části [SKU brány (starší verze SKU)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md).

###  <a name="feature"></a>Sady funkcí SKU brány

nové SKU brány Hello zjednodušit sady funkcí hello nabízí na branách hello:

| **SKU**| **Funkce**|
| ---    | ---         |
|**Basic**   | **Síť VPN založená na směrování:** 10 tunelů s P2S<br><br>**Síť VPN založená na zásadách (IKEv1):** 1 tunel, bez P2S|
| **VpnGw1, VpnGw2 a VpnGw3** | **Sítě VPN založené na trasách**: až too30 tunely (*), P2S, protokol BGP, aktivní aktivní, vlastní zásady protokolu IPsec/IKE, ExpressRoute nebo VPN koexistenci |
|        |             |

(*) Můžete nakonfigurovat tooconnect "PolicyBasedTrafficSelectors" založená na trasách zařízení brány VPN (VpnGw1, VpnGw2, VpnGw3) toomultiple místní brány firewall na základě zásad. Odkazovat příliš[toomultiple připojení VPN Gateway místně na základě zásad zařízení VPN pomocí prostředí PowerShell](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md) podrobnosti.

###  <a name="resize"></a>Změna velikosti SKU brány

1. Můžete měnit velikost mezi VpnGw1, VpnGw2 a VpnGw3 SKU.
2. Při práci s původním SKU brány hello, můžete změnit velikost mezi Basic, Standard a HighPerformance SKU.
2. Můžete **nelze** změnit velikost z SKU Basic nebo Standard nebo HighPerformance toohello nové VpnGw1/VpnGw2/VpnGw3 SKU. Místo toho musíte [migrovat](#migrate) toohello nové SKU.

###  <a name="migrate"></a>Migrace z původního toohello SKU nové SKU

[!INCLUDE [Migrate SKU](./vpn-gateway-migrate-legacy-sku-include.md)]
