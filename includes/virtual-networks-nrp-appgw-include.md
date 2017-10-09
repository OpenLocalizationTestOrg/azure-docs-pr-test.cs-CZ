## <a name="application-gateway"></a><span data-ttu-id="cc560-101">Application Gateway</span><span class="sxs-lookup"><span data-stu-id="cc560-101">Application Gateway</span></span>
<span data-ttu-id="cc560-102">Application Gateway poskytuje vyrovnávání řešení v závislosti na Vyrovnávání zatížení vrstvy 7 Azure spravované HTTP zatížení.</span><span class="sxs-lookup"><span data-stu-id="cc560-102">Application Gateway provides an Azure-managed HTTP load balancing solution based on layer 7 load balancing.</span></span> <span data-ttu-id="cc560-103">Vyrovnávání zatížení aplikací umožňuje použití hello pravidla směrování pro provoz v síti založené na protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="cc560-103">Application load balancing allows hello use of routing rules for network traffic based on HTTP.</span></span> 
<BR>

| <span data-ttu-id="cc560-104">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="cc560-104">Property</span></span> | <span data-ttu-id="cc560-105">Popis</span><span class="sxs-lookup"><span data-stu-id="cc560-105">Description</span></span> |
| --- | --- |
| <span data-ttu-id="cc560-106">**backendAddressPools**</span><span class="sxs-lookup"><span data-stu-id="cc560-106">**backendAddressPools**</span></span> |<span data-ttu-id="cc560-107">Hello seznam IP adres hello back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="cc560-107">hello list of IP addresses of hello back end servers.</span></span> <span data-ttu-id="cc560-108">uvedené Hello IP adresy by měly buď patřit toohello podsíť virtuální sítě, nebo musí být veřejné nebo virtuálními IP Adresami nebo privátní IP</span><span class="sxs-lookup"><span data-stu-id="cc560-108">hello IP addresses listed should either belong toohello virtual network subnet, or should be a public IP/VIP or private IP</span></span> |
| <span data-ttu-id="cc560-109">**backendHttpSettingsCollection**</span><span class="sxs-lookup"><span data-stu-id="cc560-109">**backendHttpSettingsCollection**</span></span> |<span data-ttu-id="cc560-110">Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="cc560-110">Every pool has settings like port, protocol, and cookie based affinity.</span></span> <span data-ttu-id="cc560-111">Tato nastavení jsou vázané tooa fond a jsou použité tooall servery v rámci fondu hello</span><span class="sxs-lookup"><span data-stu-id="cc560-111">These settings are tied tooa pool and are applied tooall servers within hello pool</span></span> |
| <span data-ttu-id="cc560-112">**frontendPorts**</span><span class="sxs-lookup"><span data-stu-id="cc560-112">**frontendPorts**</span></span> |<span data-ttu-id="cc560-113">Toto je veřejný port hello otevírá ve hello application gateway.</span><span class="sxs-lookup"><span data-stu-id="cc560-113">This port is hello public port opened on hello application gateway.</span></span> <span data-ttu-id="cc560-114">Provoz volá Tenhle port a potom získá přesměruje tooone hello back-end serverů</span><span class="sxs-lookup"><span data-stu-id="cc560-114">Traffic hits this port, and then gets redirected tooone of hello back end servers</span></span> |
| <span data-ttu-id="cc560-115">**httpListeners**</span><span class="sxs-lookup"><span data-stu-id="cc560-115">**httpListeners**</span></span> |<span data-ttu-id="cc560-116">Naslouchací proces má front-end port, protokol (Http nebo Https, ty jsou malá a velká písmena) a název certifikátu SSL hello (Pokud se konfiguruje přesměrování zpracování SSL)</span><span class="sxs-lookup"><span data-stu-id="cc560-116">Listener has a frontend port, a protocol (Http or Https, these are case-sensitive), and hello SSL certificate name (if configuring SSL offload)</span></span> |
| <span data-ttu-id="cc560-117">**requestRoutingRules**</span><span class="sxs-lookup"><span data-stu-id="cc560-117">**requestRoutingRules**</span></span> |<span data-ttu-id="cc560-118">Hello pravidlo váže hello naslouchací proces a hello back-end serveru fondu a definuje, které back-end serveru fondu hello provoz směrovat.</span><span class="sxs-lookup"><span data-stu-id="cc560-118">hello rule binds hello listener and hello back end server pool and defines which back end server pool hello traffic should be directed.</span></span> <span data-ttu-id="cc560-119">Aktuálně lze použít pouze jako kruhového dotazování</span><span class="sxs-lookup"><span data-stu-id="cc560-119">Currently works only as Round-robin</span></span> |

<span data-ttu-id="cc560-120">Příklad šablony Json brány aplikace:</span><span class="sxs-lookup"><span data-stu-id="cc560-120">Example of an application gateway Json template:</span></span>

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "location": {
          "type": "string",
          "metadata": {
            "description": "Location toodeploy to"
          }
        },
        "addressPrefix": {
          "type": "string",
          "defaultValue": "10.0.0.0/16",
          "metadata": {
            "description": "Address prefix for hello Virtual Network"
          }
        },
        "subnetPrefix": {
          "type": "string",
          "defaultValue": "10.0.0.0/28",
          "metadata": {
            "description": "Subnet prefix"
          }
        },
        "skuName": {
          "type": "string",
          "allowedValues": [
            "Standard_Small",
            "Standard_Medium",
            "Standard_Large"
          ],
          "defaultValue": "Standard_Medium",
          "metadata": {
            "description": "Sku Name"
          }
        },
        "capacity": {
          "type": "int",
          "defaultValue": 2,
          "metadata": {
            "description": "Number of instances"
          }
        },
        "backendIpAddress1": {
          "type": "string",
          "metadata": {
            "description": "IP Address for Backend Server 1"
          }
        },
        "backendIpAddress2": {
          "type": "string",
          "metadata": {
            "description": "IP Address for Backend Server 2"
          }
        }
      },
      "variables": {
        "applicationGatewayName": "applicationGateway1",
        "publicIPAddressName": "publicIp1",
        "virtualNetworkName": "virtualNetwork1",
        "subnetName": "appGatewaySubnet",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
        "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]",
        "publicIPRef": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]",
        "applicationGatewayID": "[resourceId('Microsoft.Network/applicationGateways',variables('applicationGatewayName'))]",
        "apiVersion": "2015-05-01-preview"
      },
      "resources": [
        {
          "apiVersion": "[variables('apiVersion')]",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "[variables('publicIPAddressName')]",
          "location": "[parameters('location')]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic"
          }
        },
        {
          "apiVersion": "[variables('apiVersion')]",
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
          "apiVersion": "[variables('apiVersion')]",
          "name": "[variables('applicationGatewayName')]",
          "type": "Microsoft.Network/applicationGateways",
          "location": "[parameters('location')]",
          "dependsOn": [
            "[concat('Microsoft.Network/virtualNetworks/', variables    ('virtualNetworkName'))]",
            "[concat('Microsoft.Network/publicIPAddresses/', variables    ('publicIPAddressName'))]"
          ],
          "properties": {
            "sku": {
              "name": "[parameters('skuName')]",
              "tier": "Standard",
              "capacity": "[parameters('capacity')]"
            },
            "gatewayIPConfigurations": [
              {
                "name": "appGatewayIpConfig",
                "properties": {
                  "subnet": {
                    "id": "[variables('subnetRef')]"
                  }
                }
              }
            ],
            "frontendIPConfigurations": [
              {
                "name": "appGatewayFrontendIP",
                "properties": {
                  "PublicIPAddress": {
                    "id": "[variables('publicIPRef')]"
                  }
                }
              }
            ],
            "frontendPorts": [
              {
                "name": "appGatewayFrontendPort",
                "properties": {
                  "Port": 80
                }
              }
            ],
            "backendAddressPools": [
              {
                "name": "appGatewayBackendPool",
                "properties": {
                  "BackendAddresses": [
                    {
                      "IpAddress": "[parameters('backendIpAddress1')]"
                    },
                    {
                      "IpAddress": "[parameters('backendIpAddress2')]"
                    }
                  ]
                }
              }
            ],
            "backendHttpSettingsCollection": [
              {
                "name": "appGatewayBackendHttpSettings",
                "properties": {
                  "Port": 80,
                  "Protocol": "Http",
                  "CookieBasedAffinity": "Disabled"
                }
              }
            ],
            "httpListeners": [
              {
                "name": "appGatewayHttpListener",
                "properties": {
                  "FrontendIPConfiguration": {
                    "Id": "[concat(variables('applicationGatewayID'), '/    frontendIPConfigurations/appGatewayFrontendIP')]"
                  },
                  "FrontendPort": {
                    "Id": "[concat(variables('applicationGatewayID'), '/    frontendPorts/appGatewayFrontendPort')]"
                  },
                  "Protocol": "Http",
                  "SslCertificate": null
                }
              }
            ],
            "requestRoutingRules": [
              {
                "Name": "rule1",
                "properties": {
                  "RuleType": "Basic",
                  "httpListener": {
                    "id": "[concat(variables('applicationGatewayID'), '/    httpListeners/appGatewayHttpListener')]"
                  },
                  "backendAddressPool": {
                    "id": "[concat(variables('applicationGatewayID'), '/    backendAddressPools/appGatewayBackendPool')]"
                  },
                  "backendHttpSettings": {
                    "id": "[concat(variables('applicationGatewayID'), '/    backendHttpSettingsCollection/    appGatewayBackendHttpSettings')]"
                  }
                }
              }
            ]
          }
        }
      ]    
    }


### <a name="additional-resources"></a><span data-ttu-id="cc560-121">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="cc560-121">Additional resources</span></span>
<span data-ttu-id="cc560-122">Čtení [ Aplikační brána rozhraní API REST](https://msdn.microsoft.com/library/azure/mt299388.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="cc560-122">Read [ application gateway REST API](https://msdn.microsoft.com/library/azure/mt299388.aspx) for more information.</span></span>

