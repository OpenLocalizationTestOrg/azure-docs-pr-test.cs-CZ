## <a name="load-balancer"></a><span data-ttu-id="0782d-101">Load Balancer</span><span class="sxs-lookup"><span data-stu-id="0782d-101">Load Balancer</span></span>
<span data-ttu-id="0782d-102">Nástroj pro vyrovnávání zatížení se používá, když chcete tooscale, vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="0782d-102">A load balancer is used when you want tooscale your applications.</span></span> <span data-ttu-id="0782d-103">Typické nasazení scénáře zahrnují aplikací běžících na více instancí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0782d-103">Typical deployment scenarios involve applications running on multiple VM instances.</span></span> <span data-ttu-id="0782d-104">Nástroj pro vyrovnávání zatížení, která pomáhá toodistribute síťový provoz toohello různé instance jsou přední stěnou Hello instance virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0782d-104">hello VM instances are fronted by a load balancer that helps toodistribute network traffic toohello various instances.</span></span> 

![Karty síťového rozhraní na jeden virtuální počítač](./media/resource-groups-networking/figure8.png)

| <span data-ttu-id="0782d-106">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="0782d-106">Property</span></span> | <span data-ttu-id="0782d-107">Popis</span><span class="sxs-lookup"><span data-stu-id="0782d-107">Description</span></span> |
| --- | --- |
| <span data-ttu-id="0782d-108">*frontendIPConfigurations*</span><span class="sxs-lookup"><span data-stu-id="0782d-108">*frontendIPConfigurations*</span></span> |<span data-ttu-id="0782d-109">Nástroj pro vyrovnávání zatížení může obsahovat jednu nebo více adres IP front-endu známé jako virtuální IP adresy (VIP).</span><span class="sxs-lookup"><span data-stu-id="0782d-109">a Load balancer can include one or more front end IP addresses, otherwise known as a virtual IPs (VIPs).</span></span> <span data-ttu-id="0782d-110">Tyto IP adresy sloužit jako příchozího provozu hello a může být veřejné IP adresy nebo privátní IP</span><span class="sxs-lookup"><span data-stu-id="0782d-110">These IP addresses serve as ingress for hello traffic and can be public IP or private IP</span></span> |
| <span data-ttu-id="0782d-111">*backendAddressPools*</span><span class="sxs-lookup"><span data-stu-id="0782d-111">*backendAddressPools*</span></span> |<span data-ttu-id="0782d-112">Toto jsou IP adresy přidružené hello zatížení toowhich síťové adaptéry virtuálního počítače budou distribuována.</span><span class="sxs-lookup"><span data-stu-id="0782d-112">these are IP addresses associated with hello VM NICs toowhich load will be distributed</span></span> |
| <span data-ttu-id="0782d-113">*Pravidla*</span><span class="sxs-lookup"><span data-stu-id="0782d-113">*loadBalancingRules*</span></span> |<span data-ttu-id="0782d-114">Vlastnost rule mapuje danou front-end IP a portu kombinace tooa sadu back-end IP adresy a portu kombinaci.</span><span class="sxs-lookup"><span data-stu-id="0782d-114">a rule property maps a given front end IP and port combination tooa set of back end IP addresses and port combination.</span></span> <span data-ttu-id="0782d-115">S definicí jeden prostředek pro vyrovnávání zatížení můžete definovat více pravidel Vyrovnávání zatížení, každé pravidlo odrážející kombinaci popředí end IP adresy a portu a zpět Koncová IP adresa a port přidružený virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0782d-115">With a single definition of a load balancer resource, you can define multiple load balancing rules, each rule reflecting a combination of a front end IP and port and back end IP and port associated with virtual machines.</span></span> <span data-ttu-id="0782d-116">pravidlo Hello je jeden port v hello front-endu fondu toomany virtuální počítače ve fondu back-end hello</span><span class="sxs-lookup"><span data-stu-id="0782d-116">hello rule is one port in hello front end pool toomany virtual machines in hello back end pool</span></span> |
| <span data-ttu-id="0782d-117">*Sondy*</span><span class="sxs-lookup"><span data-stu-id="0782d-117">*Probes*</span></span> |<span data-ttu-id="0782d-118">sondy umožňují sledovat tookeep hello stav instancí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0782d-118">probes enable you tookeep track of hello health of VM instances.</span></span> <span data-ttu-id="0782d-119">Pokud selže test stavu, hello instanci virtuálního počítače se provedou mimo otočení automaticky</span><span class="sxs-lookup"><span data-stu-id="0782d-119">If a health probe fails, hello virtual machine instance will be taken out of rotation automatically</span></span> |
| <span data-ttu-id="0782d-120">*inboundNatRules*</span><span class="sxs-lookup"><span data-stu-id="0782d-120">*inboundNatRules*</span></span> |<span data-ttu-id="0782d-121">Definování hello pravidla NAT příchozí provoz v IP front-endu hello a distribuovaných toohello back-end IP tooa konkrétní virtuální počítač instance.</span><span class="sxs-lookup"><span data-stu-id="0782d-121">NAT rules defining hello inbound traffic flowing through hello front end IP and distributed toohello back end IP tooa specific virtual machine instance.</span></span> <span data-ttu-id="0782d-122">Pravidlo NAT je jeden port v hello front-endu fondu tooone virtuálního počítače ve fondu back-end hello</span><span class="sxs-lookup"><span data-stu-id="0782d-122">NAT rule is one port in hello front end pool tooone virtual machine in hello back end pool</span></span> |

<span data-ttu-id="0782d-123">Příklad šablony služby Vyrovnávání zatížení ve formátu Json:</span><span class="sxs-lookup"><span data-stu-id="0782d-123">Example of load balancer template in Json format:</span></span>

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

### <a name="additional-resources"></a><span data-ttu-id="0782d-124">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="0782d-124">Additional resources</span></span>
<span data-ttu-id="0782d-125">Čtení [REST API nástroj pro vyrovnávání zatížení](https://msdn.microsoft.com/library/azure/mt163651.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="0782d-125">Read [load balancer REST API](https://msdn.microsoft.com/library/azure/mt163651.aspx) for more information.</span></span>

