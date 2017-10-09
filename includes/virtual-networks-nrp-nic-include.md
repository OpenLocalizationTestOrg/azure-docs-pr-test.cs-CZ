## <a name="nic"></a>SÍŤOVÝ ADAPTÉR
Karta (NIC) prostředku síťového rozhraní poskytuje síťové připojení tooan existující podsíť v prostředku virtuální sítě. I když můžete vytvořit síťový adaptér jako samostatný objekt, je nutné tooassociate ho tooanother objekt tooactually poskytovat připojení. Síťový adaptér může být použité tooconnect tooa podsíť virtuálních počítačů, veřejnou IP adresu nebo Vyrovnávání zatížení.  

| Vlastnost | Popis | Ukázkové hodnoty |
| --- | --- | --- |
| **virtuální počítač** |Virtuální počítač hello síťový adaptér je přidružen. |/subscriptions/{GUID}/../microsoft.COMPUTE/virtualMachines/vm1 |
| **macAddress** |Adresa MAC pro síťový adaptér hello |Libovolná hodnota od 4 do 30. |
| **skupinu zabezpečení sítě** |Toohello NSG přidruženou síťovou kartu |/subscriptions/{GUID}/../microsoft.Network/networkSecurityGroups/myNSG1 |
| **dnsSettings** |Nastavení DNS pro hello síťový adaptér |v tématu [PIP](#Public-IP-address) |

Síťový adaptér nebo síťový adaptér představuje síťové rozhraní, které může být přidružené tooa virtuální počítač (VM). Virtuální počítač může mít jeden nebo více síťových adaptérů.

![Karty síťového rozhraní na jeden virtuální počítač](./media/resource-groups-networking/Figure3.png)

### <a name="ip-configurations"></a>Konfigurace protokolu IP
Síťové adaptéry mít podřízený objekt s názvem **ipConfigurations** obsahující hello následující vlastnosti:

| Vlastnost | Popis | Ukázkové hodnoty |
| --- | --- | --- |
| **podsíť** |Podsíť hello síťový adaptér je onnected k. |/subscriptions/{GUID}/../microsoft.Network/virtualNetworks/myvnet1/subnets/mysub1 |
| **privateIPAddress** |IP adresa pro hello síťový adaptér v podsíti hello |10.0.0.8 |
| **privateIPAllocationMethod** |Metoda přidělení IP |Dynamická nebo statická |
| **enableIPForwarding** |Jestli se dá použít hello síťový adaptér pro směrování |hodnotu true nebo false |
| **primární** |Jestli je hello síťový adaptér hello primární síťový adaptér pro hello virtuálních počítačů |hodnotu true nebo false |
| **publicIPAddress** |PIP přidružené hello síťový adaptér |v tématu [nastavení DNS](#DNS-settings) |
| **pravidlo loadBalancerBackendAddressPools** |Back end adresu fondy hello síťový adaptér je spojené s | |
| **loadBalancerInboundNatRules** |Příchozí zatížení vyrovnávání NAT pravidla hello síťový adaptér je spojené s | |

Ukázka veřejnou IP adresu ve formátu JSON:

    {
        "name": "lb-nic1-be",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/networkInterfaces",
        "location": "eastus",
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "ipConfigurations": [
                {
                    "name": "NIC-config",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/NIC-config",
                    "etag": "W/\"0027f1a2-3ac8-49de-b5d5-fd46550500b1\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "privateIPAddress": "10.0.0.4",
                        "privateIPAllocationMethod": "Dynamic",
                        "subnet": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet"
                        },
                        "loadBalancerBackendAddressPools": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool"
                            }
                        ],
                        "loadBalancerInboundNatRules": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1"
                            }
                        ]
                    }
                }
            ],
            "dnsSettings": { ... },
            "macAddress": "00-0D-3A-10-F1-29",
            "enableIPForwarding": false,
            "primary": true,
            "virtualMachine": {
                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Compute/virtualMachines/web1"
            }
        }
    }

### <a name="additional-resources"></a>Další zdroje
* Čtení hello [referenční dokumentace rozhraní API REST](https://msdn.microsoft.com/library/azure/mt163579.aspx) pro síťové adaptéry.

