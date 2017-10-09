## <a name="route-tables"></a>Směrovací tabulky
Prostředky tabulky trasy obsahuje tras toodefine tok provozu v rámci infrastruktury Azure. Můžete vytvořit uživatelsky definované trasy (UDR) toosend veškerý provoz z dané podsíti tooa virtuální zařízení, například Brána firewall nebo neoprávněných vniknutí systém detekce (ID). Můžete přidružit toosubnets tabulka trasy. 

Směrovací tabulky obsahují hello následující vlastnosti.

| Vlastnost | Popis | Ukázkové hodnoty |
| --- | --- | --- |
| **trasy** |Kolekce uživatelem definovaných tras ve směrovací tabulce hello |v tématu [trasy definované uživatelem](#User-defined-routes) |
| **podsítě** |Kolekce podsítě hello směrovací tabulka je příliš použít|v tématu [podsítě](#Subnets) |

### <a name="user-defined-routes"></a>Trasy definované uživatelem
Udr toospecify, kde má být odeslán provoz, můžete vytvořit na základě jeho cílové adresy. Trasa si můžete představit jako hello výchozí brány definici na základě hello cílové adresy síťového paketu.

Udr obsahovat hello následující vlastnosti. 

| Vlastnost | Popis | Ukázkové hodnoty |
| --- | --- | --- |
| **addressPrefix** |Předpona adresy nebo úplné IP adresu pro cílový hello |192.168.1.0/24, 192.168.1.101 |
| **nextHopType** |Typ provozu hello zařízení zašle příliš|VirtualAppliance, brány sítě VPN, Internet |
| **nextHopIpAddress** |IP adresa pro další segment hello |192.168.1.4 |

Ukázka směrovací tabulku ve formátu JSON:

    {
        "name": "UDR-BackEnd",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/routeTables",
        "location": "westus",
        "properties": {
            "provisioningState": "Succeeded",
            "routes": [
                {
                    "name": "RouteToFrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd/routes/RouteToFrontEnd",
                    "etag": "W/\"v\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "nextHopType": "VirtualAppliance",
                        "nextHopIpAddress": "192.168.0.4"
                    }
                }
            ],
            "subnets": [
                {
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd"
                }
            ]
        }
    }

### <a name="additional-resources"></a>Další zdroje
* Přečtěte si další informace o [udr](../articles/virtual-network/virtual-networks-udr-overview.md).
* Čtení hello [referenční dokumentace rozhraní API REST](https://msdn.microsoft.com/library/azure/mt502549.aspx) pro směrovací tabulky.
* Čtení hello [referenční dokumentace rozhraní API REST](https://msdn.microsoft.com/library/azure/mt502539.aspx) pro uživatele definované trasy (udr).

