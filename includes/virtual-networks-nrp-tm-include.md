## <a name="traffic-manager-profile"></a><span data-ttu-id="8d247-101">Profil služby Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="8d247-101">Traffic Manager Profile</span></span>
<span data-ttu-id="8d247-102">Správce provozu a její podřízené prostředku koncového bodu povolte směrování tooendpoints DNS v Azure a mimo Azure.</span><span class="sxs-lookup"><span data-stu-id="8d247-102">Traffic manager and its child endpoint resource enable DNS routing tooendpoints in Azure and outside of Azure.</span></span> <span data-ttu-id="8d247-103">Takové distribuce přenosů se řídí metody směrování zásad.</span><span class="sxs-lookup"><span data-stu-id="8d247-103">Such traffic distribution is governed by routing  policy methods.</span></span> <span data-ttu-id="8d247-104">Správce provozu také umožňuje monitorovat stav toobe koncový bod a provoz běžnými správně založený na dobrý stav koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="8d247-104">Traffic manager also allows endpoint health toobe monitored, and traffic diverted appropriately based on hello health of an endpoint.</span></span> 

| <span data-ttu-id="8d247-105">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="8d247-105">Property</span></span> | <span data-ttu-id="8d247-106">Popis</span><span class="sxs-lookup"><span data-stu-id="8d247-106">Description</span></span> |
| --- | --- |
| <span data-ttu-id="8d247-107">**trafficRoutingMethod**</span><span class="sxs-lookup"><span data-stu-id="8d247-107">**trafficRoutingMethod**</span></span> |<span data-ttu-id="8d247-108">možné hodnoty jsou *výkonu*, *vážená*, a *s prioritou*</span><span class="sxs-lookup"><span data-stu-id="8d247-108">possible values are *Performance*, *Weighted*, and *Priority*</span></span> |
| <span data-ttu-id="8d247-109">**dnsConfig**</span><span class="sxs-lookup"><span data-stu-id="8d247-109">**dnsConfig**</span></span> |<span data-ttu-id="8d247-110">Plně kvalifikovaný název domény pro profil hello</span><span class="sxs-lookup"><span data-stu-id="8d247-110">FQDN for hello profile</span></span> |
| <span data-ttu-id="8d247-111">**Protokol**</span><span class="sxs-lookup"><span data-stu-id="8d247-111">**Protocol**</span></span> |<span data-ttu-id="8d247-112">protokol pro sledování, možné hodnoty jsou *HTTP* a *HTTPS*</span><span class="sxs-lookup"><span data-stu-id="8d247-112">monitoring protocol, possible values are *HTTP* and *HTTPS*</span></span> |
| <span data-ttu-id="8d247-113">**Port**</span><span class="sxs-lookup"><span data-stu-id="8d247-113">**Port**</span></span> |<span data-ttu-id="8d247-114">monitorování portu</span><span class="sxs-lookup"><span data-stu-id="8d247-114">monitoring port</span></span> |
| <span data-ttu-id="8d247-115">**Cesta**</span><span class="sxs-lookup"><span data-stu-id="8d247-115">**Path**</span></span> |<span data-ttu-id="8d247-116">Cesta monitorování</span><span class="sxs-lookup"><span data-stu-id="8d247-116">monitoring path</span></span> |
| <span data-ttu-id="8d247-117">**Koncové body**</span><span class="sxs-lookup"><span data-stu-id="8d247-117">**Endpoints**</span></span> |<span data-ttu-id="8d247-118">kontejner pro koncový bod prostředků</span><span class="sxs-lookup"><span data-stu-id="8d247-118">container for endpoint resources</span></span> |

### <a name="endpoint"></a><span data-ttu-id="8d247-119">Koncový bod</span><span class="sxs-lookup"><span data-stu-id="8d247-119">Endpoint</span></span>
<span data-ttu-id="8d247-120">Koncový bod je podřízený prostředek profilu Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="8d247-120">An endpoint is a child resource of a Traffic Manager Profile.</span></span> <span data-ttu-id="8d247-121">Reprezentuje službu nebo webové koncový bod toowhich uživatele se provoz rozděluje na základě zásad hello nakonfigurované v hello profil služby Traffic Manager prostředků.</span><span class="sxs-lookup"><span data-stu-id="8d247-121">It represents a service or web endpoint toowhich user traffic is distributed based on hello configured policy in hello Traffic Manager Profile resource.</span></span> 

| <span data-ttu-id="8d247-122">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="8d247-122">Property</span></span> | <span data-ttu-id="8d247-123">Popis</span><span class="sxs-lookup"><span data-stu-id="8d247-123">Description</span></span> |
| --- | --- |
| <span data-ttu-id="8d247-124">**Typ**</span><span class="sxs-lookup"><span data-stu-id="8d247-124">**Type**</span></span> |<span data-ttu-id="8d247-125">Hello typ koncového bodu hello, možné hodnoty jsou *Azure koncový bod*, *externí koncový bod*, a *vnořené koncový bod*</span><span class="sxs-lookup"><span data-stu-id="8d247-125">hello type of hello endpoint, possible values are *Azure End point*, *External Endpoint*, and  *Nested Endpoint*</span></span> |
| <span data-ttu-id="8d247-126">**targetResourceId**</span><span class="sxs-lookup"><span data-stu-id="8d247-126">**targetResourceId**</span></span> |<span data-ttu-id="8d247-127">veřejná IP adresa koncového bodu služby nebo web.</span><span class="sxs-lookup"><span data-stu-id="8d247-127">public IP address of a service or web endpoint.</span></span> <span data-ttu-id="8d247-128">To může být Azure nebo externí koncový bod.</span><span class="sxs-lookup"><span data-stu-id="8d247-128">This can be an Azure or external endpoint.</span></span> |
| <span data-ttu-id="8d247-129">**Váha**</span><span class="sxs-lookup"><span data-stu-id="8d247-129">**Weight**</span></span> |<span data-ttu-id="8d247-130">koncový bod váhy použít řízení provozu.</span><span class="sxs-lookup"><span data-stu-id="8d247-130">endpoint weight used in traffic management.</span></span> |
| <span data-ttu-id="8d247-131">**Priorita**</span><span class="sxs-lookup"><span data-stu-id="8d247-131">**Priority**</span></span> |<span data-ttu-id="8d247-132">Priorita koncového bodu hello, použité toodefine akce převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="8d247-132">priority of hello endpoint, used toodefine a failover action</span></span> |

<span data-ttu-id="8d247-133">Ukázka služby Traffic Manager ve formátu Json:</span><span class="sxs-lookup"><span data-stu-id="8d247-133">Sample of Traffic Manager in Json format:</span></span> 

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


## <a name="additional-resources"></a><span data-ttu-id="8d247-134">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8d247-134">Additional resources</span></span>
<span data-ttu-id="8d247-135">Čtení [dokumentace k REST API pro správce provozu](https://msdn.microsoft.com/library/azure/mt163664.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="8d247-135">Read [REST API documentation for Traffic Manager](https://msdn.microsoft.com/library/azure/mt163664.aspx) for more information.</span></span>

