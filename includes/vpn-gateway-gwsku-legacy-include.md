Brána sítě VPN starší verze (starý) Hello SKU jsou:

* Basic
* Standard
* HighPerformance

Brána sítě VPN nebude používat skladová položka brány UltraPerformance hello. Informace o hello UltraPerformance SKU najdete v tématu hello [ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md) dokumentaci.

Při práci s hello starší verze SKU, zvažte následující hello:

* Pokud chcete toouse typu Policybased, je nutné použít hello základní SKU. Sítě VPN typu PolicyBased (dříve nazývané Statické směrování) nejsou podporovány jinými SKU.
* Protokol BGP není podporován na hello základní SKU.
* Brána ExpressRoute VPN existovat vedle sebe konfigurace nejsou podporovány v hello základní SKU.
* V hello HighPerformance SKU pouze lze nakonfigurovat připojení S2S VPN Gateway aktivní aktivní.
