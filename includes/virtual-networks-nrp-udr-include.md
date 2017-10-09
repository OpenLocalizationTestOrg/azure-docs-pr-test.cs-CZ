## <a name="route-tables"></a><span data-ttu-id="d044f-101">Směrovací tabulky</span><span class="sxs-lookup"><span data-stu-id="d044f-101">Route tables</span></span>
<span data-ttu-id="d044f-102">Prostředky tabulky trasy obsahuje tras toodefine tok provozu v rámci infrastruktury Azure.</span><span class="sxs-lookup"><span data-stu-id="d044f-102">Route table resources contains routes used toodefine how traffic flows within your Azure infrastructure.</span></span> <span data-ttu-id="d044f-103">Můžete vytvořit uživatelsky definované trasy (UDR) toosend veškerý provoz z dané podsíti tooa virtuální zařízení, například Brána firewall nebo neoprávněných vniknutí systém detekce (ID).</span><span class="sxs-lookup"><span data-stu-id="d044f-103">You can use user defined routes (UDR) toosend all traffic from a given subnet tooa virtual appliance, such as a firewall or intrusion detection system (IDS).</span></span> <span data-ttu-id="d044f-104">Můžete přidružit toosubnets tabulka trasy.</span><span class="sxs-lookup"><span data-stu-id="d044f-104">You can associate a route table toosubnets.</span></span> 

<span data-ttu-id="d044f-105">Směrovací tabulky obsahují hello následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d044f-105">Route tables contain hello following properties.</span></span>

| <span data-ttu-id="d044f-106">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="d044f-106">Property</span></span> | <span data-ttu-id="d044f-107">Popis</span><span class="sxs-lookup"><span data-stu-id="d044f-107">Description</span></span> | <span data-ttu-id="d044f-108">Ukázkové hodnoty</span><span class="sxs-lookup"><span data-stu-id="d044f-108">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d044f-109">**trasy**</span><span class="sxs-lookup"><span data-stu-id="d044f-109">**routes**</span></span> |<span data-ttu-id="d044f-110">Kolekce uživatelem definovaných tras ve směrovací tabulce hello</span><span class="sxs-lookup"><span data-stu-id="d044f-110">Collection of user defined routes in hello route table</span></span> |<span data-ttu-id="d044f-111">v tématu [trasy definované uživatelem](#User-defined-routes)</span><span class="sxs-lookup"><span data-stu-id="d044f-111">see [user defined routes](#User-defined-routes)</span></span> |
| <span data-ttu-id="d044f-112">**podsítě**</span><span class="sxs-lookup"><span data-stu-id="d044f-112">**subnets**</span></span> |<span data-ttu-id="d044f-113">Kolekce podsítě hello směrovací tabulka je příliš použít</span><span class="sxs-lookup"><span data-stu-id="d044f-113">Collection of subnets hello route table is applied too</span></span>|<span data-ttu-id="d044f-114">v tématu [podsítě](#Subnets)</span><span class="sxs-lookup"><span data-stu-id="d044f-114">see [subnets](#Subnets)</span></span> |

### <a name="user-defined-routes"></a><span data-ttu-id="d044f-115">Trasy definované uživatelem</span><span class="sxs-lookup"><span data-stu-id="d044f-115">User defined routes</span></span>
<span data-ttu-id="d044f-116">Udr toospecify, kde má být odeslán provoz, můžete vytvořit na základě jeho cílové adresy.</span><span class="sxs-lookup"><span data-stu-id="d044f-116">You can create UDRs toospecify where traffic should be sent to, based on its destination address.</span></span> <span data-ttu-id="d044f-117">Trasa si můžete představit jako hello výchozí brány definici na základě hello cílové adresy síťového paketu.</span><span class="sxs-lookup"><span data-stu-id="d044f-117">You can think of a route as hello default gateway definition based on hello destination address of a network packet.</span></span>

<span data-ttu-id="d044f-118">Udr obsahovat hello následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d044f-118">UDRs contain hello following properties.</span></span> 

| <span data-ttu-id="d044f-119">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="d044f-119">Property</span></span> | <span data-ttu-id="d044f-120">Popis</span><span class="sxs-lookup"><span data-stu-id="d044f-120">Description</span></span> | <span data-ttu-id="d044f-121">Ukázkové hodnoty</span><span class="sxs-lookup"><span data-stu-id="d044f-121">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d044f-122">**addressPrefix**</span><span class="sxs-lookup"><span data-stu-id="d044f-122">**addressPrefix**</span></span> |<span data-ttu-id="d044f-123">Předpona adresy nebo úplné IP adresu pro cílový hello</span><span class="sxs-lookup"><span data-stu-id="d044f-123">Address prefix, or full IP address for hello destination</span></span> |<span data-ttu-id="d044f-124">192.168.1.0/24, 192.168.1.101</span><span class="sxs-lookup"><span data-stu-id="d044f-124">192.168.1.0/24, 192.168.1.101</span></span> |
| <span data-ttu-id="d044f-125">**nextHopType**</span><span class="sxs-lookup"><span data-stu-id="d044f-125">**nextHopType**</span></span> |<span data-ttu-id="d044f-126">Typ provozu hello zařízení zašle příliš</span><span class="sxs-lookup"><span data-stu-id="d044f-126">Type of device hello traffic will be sent too</span></span>|<span data-ttu-id="d044f-127">VirtualAppliance, brány sítě VPN, Internet</span><span class="sxs-lookup"><span data-stu-id="d044f-127">VirtualAppliance, VPN Gateway, Internet</span></span> |
| <span data-ttu-id="d044f-128">**nextHopIpAddress**</span><span class="sxs-lookup"><span data-stu-id="d044f-128">**nextHopIpAddress**</span></span> |<span data-ttu-id="d044f-129">IP adresa pro další segment hello</span><span class="sxs-lookup"><span data-stu-id="d044f-129">IP address for hello next hop</span></span> |<span data-ttu-id="d044f-130">192.168.1.4</span><span class="sxs-lookup"><span data-stu-id="d044f-130">192.168.1.4</span></span> |

<span data-ttu-id="d044f-131">Ukázka směrovací tabulku ve formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="d044f-131">Sample route table in JSON format:</span></span>

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

### <a name="additional-resources"></a><span data-ttu-id="d044f-132">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d044f-132">Additional resources</span></span>
* <span data-ttu-id="d044f-133">Přečtěte si další informace o [udr](../articles/virtual-network/virtual-networks-udr-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d044f-133">Get more information about [UDRs](../articles/virtual-network/virtual-networks-udr-overview.md).</span></span>
* <span data-ttu-id="d044f-134">Čtení hello [referenční dokumentace rozhraní API REST](https://msdn.microsoft.com/library/azure/mt502549.aspx) pro směrovací tabulky.</span><span class="sxs-lookup"><span data-stu-id="d044f-134">Read hello [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt502549.aspx) for route tables.</span></span>
* <span data-ttu-id="d044f-135">Čtení hello [referenční dokumentace rozhraní API REST](https://msdn.microsoft.com/library/azure/mt502539.aspx) pro uživatele definované trasy (udr).</span><span class="sxs-lookup"><span data-stu-id="d044f-135">Read hello [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt502539.aspx) for user defined routes (UDRs).</span></span>

