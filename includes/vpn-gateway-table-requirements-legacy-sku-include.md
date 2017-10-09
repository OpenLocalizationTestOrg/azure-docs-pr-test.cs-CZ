Následující tabulka seznamy hello požadavky pro PolicyBased a sítě VPN RouteBased brány Hello. Tato tabulka platí tooboth hello Resource Manager a modely nasazení classic. Pro hello klasického modelu brány sítě VPN PolicyBased se hello stejné jako statické brány a brány založené na směrování jsou hello stejné jako dynamické brány.

|  | **Brána sítě VPN PolicyBased Basic** | **Brána sítě VPN RouteBased Basic** | **Brána sítě VPN RouteBased Standard** | **RouteBased vysoce výkonná brána sítě VPN** |
| --- | --- | --- | --- | --- |
| **Připojení Site-to-Site (S2S)** |Konfigurace sítě VPN PolicyBased |Konfigurace sítě VPN RouteBased |Konfigurace sítě VPN RouteBased |Konfigurace sítě VPN RouteBased |
| **Připojení typu point-to-site (P2S**) |Nepodporuje se |Podporuje se (může existovat vedle připojení S2S) |Podporuje se (může existovat vedle připojení S2S) |Podporuje se (může existovat vedle připojení S2S) |
| **Metoda ověřování** |Předsdílený klíč |Předsdílený klíč pro připojení S2S, certifikáty pro připojení P2S |Předsdílený klíč pro připojení S2S, certifikáty pro připojení P2S |Předsdílený klíč pro připojení S2S, certifikáty pro připojení P2S |
| **Maximální počet připojení S2S** |1 |10 |10 |30 |
| **Maximální počet připojení P2S** |Nepodporuje se |128 |128 |128 |
| **Podpora aktivního směrování (BGP)** |Nepodporuje se |Nepodporuje se |Podporuje se |Podporuje se |

