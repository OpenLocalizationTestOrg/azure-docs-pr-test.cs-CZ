## <a name="load-balancer"></a>Load Balancer
Nástroj pro vyrovnávání zatížení se používá, když chcete tooscale, vaše aplikace. Typické nasazení scénáře zahrnují aplikací běžících na více instancí virtuálního počítače. Nástroj pro vyrovnávání zatížení, která pomáhá toodistribute síťový provoz toohello různé instance jsou přední stěnou Hello instance virtuálních počítačů. 

![Karty síťového rozhraní na jeden virtuální počítač](./media/resource-groups-networking/figure8.png)

| Vlastnost | Popis |
| --- | --- |
| *frontendIPConfigurations* |Nástroj pro vyrovnávání zatížení může obsahovat jednu nebo více adres IP front-endu známé jako virtuální IP adresy (VIP). Tyto IP adresy sloužit jako příchozího provozu hello a může být veřejné IP adresy nebo privátní IP |
| *backendAddressPools* |Toto jsou IP adresy přidružené hello zatížení toowhich síťové adaptéry virtuálního počítače budou distribuována. |
| *Pravidla* |Vlastnost rule mapuje danou front-end IP a portu kombinace tooa sadu back-end IP adresy a portu kombinaci. S definicí jeden prostředek pro vyrovnávání zatížení můžete definovat více pravidel Vyrovnávání zatížení, každé pravidlo odrážející kombinaci popředí end IP adresy a portu a zpět Koncová IP adresa a port přidružený virtuálních počítačů. pravidlo Hello je jeden port v hello front-endu fondu toomany virtuální počítače ve fondu back-end hello |
| *Sondy* |sondy umožňují sledovat tookeep hello stav instancí virtuálních počítačů. Pokud selže test stavu, hello instanci virtuálního počítače se provedou mimo otočení automaticky |
| *inboundNatRules* |Definování hello pravidla NAT příchozí provoz v IP front-endu hello a distribuovaných toohello back-end IP tooa konkrétní virtuální počítač instance. Pravidlo NAT je jeden port v hello front-endu fondu tooone virtuálního počítače ve fondu back-end hello |

Příklad šablony služby Vyrovnávání zatížení ve formátu Json:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "dnsNameforLBIP": {
          "type": "string",
          "metadata": {
            "description": "Unique DNS name"
          }
        },
        "location": {
          "type": "string",
          "allowedValues": [
            "East US",
            "West US",
            "West Europe",
            "East Asia",
            "Southeast Asia"
          ],
          "metadata": {
            "description": "Location toodeploy"
          }
        },
        "addressPrefix": {
          "type": "string",
          "defaultValue": "10.0.0.0/16",
          "metadata": {
            "description": "Address Prefix"
          }
        },
        "subnetPrefix": {
          "type": "string",
          "defaultValue": "10.0.0.0/24",
          "metadata": {
            "description": "Subnet Prefix"
          }
        },
        "publicIPAddressType": {
          "type": "string",
          "defaultValue": "Dynamic",
          "allowedValues": [
            "Dynamic",
            "Static"
          ],
          "metadata": {
            "description": "Public IP type"
          }
        }
      },
      "variables": {
        "virtualNetworkName": "virtualNetwork1",
        "publicIPAddressName": "publicIp1",
        "subnetName": "subnet1",
        "loadBalancerName": "loadBalancer1",
        "nicName": "networkInterface1",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
        "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]",
        "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
        "nicId": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
        "backEndIPConfigID": "[concat(variables('nicId'),'/ipConfigurations/ipconfig1')]"
      },
      "resources": [
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIPAddressName')]",
      "location": "[parameters('location')]",
      "properties": {
        "publicIPAllocationMethod": "[parameters('publicIPAddressType')]",
        "dnsSettings": {
          "domainNameLabel": "[parameters('dnsNameforLBIP')]"
        }
      }
    },
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[parameters('location')]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[parameters('addressPrefix')]"
          ]
        },
        "subnets": [
          {
            "name": "[variables('subnetName')]",
            "properties": {
              "addressPrefix": "[parameters('subnetPrefix')]"
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "subnet": {
                "id": "[variables('subnetRef')]"
              },
              "loadBalancerBackendAddressPools": [
                {
                  "id": "[concat(variables('lbID'), '/backendAddressPools/LoadBalancerBackend')]"
                }
              ],
              "loadBalancerInboundNatRules": [
                {
                  "id": "[concat(variables('lbID'),'/inboundNatRules/RDP')]"
                }
              ]
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-05-01-preview",
      "name": "[variables('loadBalancerName')]",
      "type": "Microsoft.Network/loadBalancers",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]"
      ],
      "properties": {
        "frontendIPConfigurations": [
          {
            "name": "loadBalancerFrontEnd",
            "properties": {
              "publicIPAddress": {
                "id": "[variables('publicIPAddressID')]"
              }
            }
          }
        ],
        "backendAddressPools": [
          {
            "name": "loadBalancerBackEnd"
          }
        ],
        "inboundNatRules": [
          {
            "name": "RDP",
            "properties": {
              "frontendIPConfiguration": {
                "id": "[variables('frontEndIPConfigID')]"
              },
              "protocol": "tcp",
              "frontendPort": 3389,
              "backendPort": 3389,
              "enableFloatingIP": false
            }
          }
        ]
      }
    }
      ]
    }

### <a name="additional-resources"></a>Další zdroje
Čtení [REST API nástroj pro vyrovnávání zatížení](https://msdn.microsoft.com/library/azure/mt163651.aspx) Další informace.

