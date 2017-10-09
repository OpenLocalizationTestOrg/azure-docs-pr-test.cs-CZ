## <a name="traffic-manager-profile"></a>Profil služby Traffic Manager
Správce provozu a její podřízené prostředku koncového bodu povolte směrování tooendpoints DNS v Azure a mimo Azure. Takové distribuce přenosů se řídí metody směrování zásad. Správce provozu také umožňuje monitorovat stav toobe koncový bod a provoz běžnými správně založený na dobrý stav koncového bodu. 

| Vlastnost | Popis |
| --- | --- |
| **trafficRoutingMethod** |možné hodnoty jsou *výkonu*, *vážená*, a *s prioritou* |
| **dnsConfig** |Plně kvalifikovaný název domény pro profil hello |
| **Protokol** |protokol pro sledování, možné hodnoty jsou *HTTP* a *HTTPS* |
| **Port** |monitorování portu |
| **Cesta** |Cesta monitorování |
| **Koncové body** |kontejner pro koncový bod prostředků |

### <a name="endpoint"></a>Koncový bod
Koncový bod je podřízený prostředek profilu Traffic Manageru. Reprezentuje službu nebo webové koncový bod toowhich uživatele se provoz rozděluje na základě zásad hello nakonfigurované v hello profil služby Traffic Manager prostředků. 

| Vlastnost | Popis |
| --- | --- |
| **Typ** |Hello typ koncového bodu hello, možné hodnoty jsou *Azure koncový bod*, *externí koncový bod*, a *vnořené koncový bod* |
| **targetResourceId** |veřejná IP adresa koncového bodu služby nebo web. To může být Azure nebo externí koncový bod. |
| **Váha** |koncový bod váhy použít řízení provozu. |
| **Priorita** |Priorita koncového bodu hello, použité toodefine akce převzetí služeb při selhání |

Ukázka služby Traffic Manager ve formátu Json: 

        {
            "apiVersion": "[variables('tmApiVersion')]",
            "type": "Microsoft.Network/trafficManagerProfiles",
            "name": "VMEndpointExample",
            "location": "global",
            "dependsOn": [
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '0')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '1')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '2')]",
            ],
            "properties": {
                "profileStatus": "Enabled",
                "trafficRoutingMethod": "Weighted",
                "dnsConfig": {
                    "relativeName": "[parameters('dnsname')]",
                    "ttl": 30
                },
                "monitorConfig": {
                    "protocol": "http",
                    "port": 80,
                    "path": "/"
                },
                "endpoints": [
                    {
                        "name": "endpoint0",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 0))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint1",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 1))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint2",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 2))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    }
                ]
            }
        }


## <a name="additional-resources"></a>Další zdroje
Čtení [dokumentace k REST API pro správce provozu](https://msdn.microsoft.com/library/azure/mt163664.aspx) Další informace.

