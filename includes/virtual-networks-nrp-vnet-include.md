## <a name="virtual-network"></a><span data-ttu-id="69b63-101">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="69b63-101">Virtual Network</span></span>
<span data-ttu-id="69b63-102">Virtuální sítí (VNET) a podsítě prostředky pomohl definovat hranici zabezpečení pro úlohy běžící v Azure.</span><span class="sxs-lookup"><span data-stu-id="69b63-102">Virtual Networks (VNET) and subnets resources help define a security boundary for workloads running in Azure.</span></span> <span data-ttu-id="69b63-103">Virtuální síť je charakterizovaná kolekce adresní prostory, které jsou definované jako bloků CIDR.</span><span class="sxs-lookup"><span data-stu-id="69b63-103">A VNet is characterized by a collection of address spaces, defined as CIDR blocks.</span></span> 

> [!NOTE]
> <span data-ttu-id="69b63-104">Správci sítě se seznámíte s notaci CIDR.</span><span class="sxs-lookup"><span data-stu-id="69b63-104">Network administrators are familiar with CIDR notation.</span></span> <span data-ttu-id="69b63-105">Pokud nejste obeznámeni s CIDR, [Další informace o](http://whatismyipaddress.com/cidr).</span><span class="sxs-lookup"><span data-stu-id="69b63-105">If you are not familiar with CIDR, [learn more about it](http://whatismyipaddress.com/cidr).</span></span>
> 
> 

![Virtuální síť s více podsítěmi](./media/resource-groups-networking/Figure4.png)

<span data-ttu-id="69b63-107">Virtuální sítě obsahovat hello následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="69b63-107">VNets contain hello following properties.</span></span>

| <span data-ttu-id="69b63-108">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="69b63-108">Property</span></span> | <span data-ttu-id="69b63-109">Popis</span><span class="sxs-lookup"><span data-stu-id="69b63-109">Description</span></span> | <span data-ttu-id="69b63-110">Ukázkové hodnoty</span><span class="sxs-lookup"><span data-stu-id="69b63-110">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="69b63-111">**adresní prostor**</span><span class="sxs-lookup"><span data-stu-id="69b63-111">**addressSpace**</span></span> |<span data-ttu-id="69b63-112">Kolekce předpon adres, které tvoří hello virtuální sítě v notaci CIDR</span><span class="sxs-lookup"><span data-stu-id="69b63-112">Collection of address prefixes that make up hello VNet in CIDR notation</span></span> |<span data-ttu-id="69b63-113">192.168.0.0/16</span><span class="sxs-lookup"><span data-stu-id="69b63-113">192.168.0.0/16</span></span> |
| <span data-ttu-id="69b63-114">**podsítě**</span><span class="sxs-lookup"><span data-stu-id="69b63-114">**subnets**</span></span> |<span data-ttu-id="69b63-115">Kolekce podsítě, které tvoří hello virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="69b63-115">Collection of subnets that make up hello VNet</span></span> |<span data-ttu-id="69b63-116">v tématu [podsítě](#Subnets) níže.</span><span class="sxs-lookup"><span data-stu-id="69b63-116">see [subnets](#Subnets) below.</span></span> |
| <span data-ttu-id="69b63-117">**IP adresa**</span><span class="sxs-lookup"><span data-stu-id="69b63-117">**ipAddress**</span></span> |<span data-ttu-id="69b63-118">Tooobject přidělit IP adresu.</span><span class="sxs-lookup"><span data-stu-id="69b63-118">IP address assigned tooobject.</span></span> <span data-ttu-id="69b63-119">Toto je vlastnost jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="69b63-119">This is a read-only property.</span></span> |<span data-ttu-id="69b63-120">104.42.233.77</span><span class="sxs-lookup"><span data-stu-id="69b63-120">104.42.233.77</span></span> |

### <a name="subnets"></a><span data-ttu-id="69b63-121">Podsítě</span><span class="sxs-lookup"><span data-stu-id="69b63-121">Subnets</span></span>
<span data-ttu-id="69b63-122">Podsíť je prostředkem podřízené virtuální sítě, a pomáhá definovat segmenty adresní prostory v rámci blok CIDR pomocí předpony IP adres.</span><span class="sxs-lookup"><span data-stu-id="69b63-122">A subnet is a child resource of a VNet, and helps define segments of address spaces within a CIDR block, using IP address prefixes.</span></span> <span data-ttu-id="69b63-123">Síťové adaptéry lze přidat toosubnets a připojené tooVMs, poskytuje připojení pro různé úlohy.</span><span class="sxs-lookup"><span data-stu-id="69b63-123">NICs can be added toosubnets, and connected tooVMs, providing connectivity for various workloads.</span></span>

<span data-ttu-id="69b63-124">Podsítě obsahovat hello následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="69b63-124">Subnets contain hello following properties.</span></span> 

| <span data-ttu-id="69b63-125">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="69b63-125">Property</span></span> | <span data-ttu-id="69b63-126">Popis</span><span class="sxs-lookup"><span data-stu-id="69b63-126">Description</span></span> | <span data-ttu-id="69b63-127">Ukázkové hodnoty</span><span class="sxs-lookup"><span data-stu-id="69b63-127">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="69b63-128">**addressPrefix**</span><span class="sxs-lookup"><span data-stu-id="69b63-128">**addressPrefix**</span></span> |<span data-ttu-id="69b63-129">Jedna adresa předponu, která tvoří hello podsíť v notaci CIDR</span><span class="sxs-lookup"><span data-stu-id="69b63-129">Single address prefix that make up hello subnet in CIDR notation</span></span> |<span data-ttu-id="69b63-130">192.168.1.0/24</span><span class="sxs-lookup"><span data-stu-id="69b63-130">192.168.1.0/24</span></span> |
| <span data-ttu-id="69b63-131">**skupinu zabezpečení sítě**</span><span class="sxs-lookup"><span data-stu-id="69b63-131">**networkSecurityGroup**</span></span> |<span data-ttu-id="69b63-132">Skupina NSG použitá toohello podsítě</span><span class="sxs-lookup"><span data-stu-id="69b63-132">NSG applied toohello subnet</span></span> |<span data-ttu-id="69b63-133">v tématu [skupiny Nsg](#Network-Security-Group)</span><span class="sxs-lookup"><span data-stu-id="69b63-133">see [NSGs](#Network-Security-Group)</span></span> |
| <span data-ttu-id="69b63-134">**routeTable**</span><span class="sxs-lookup"><span data-stu-id="69b63-134">**routeTable**</span></span> |<span data-ttu-id="69b63-135">Směrovací tabulka použita toohello podsítě</span><span class="sxs-lookup"><span data-stu-id="69b63-135">Route table applied toohello subnet</span></span> |<span data-ttu-id="69b63-136">v tématu [UDR](#Route-table)</span><span class="sxs-lookup"><span data-stu-id="69b63-136">see [UDR](#Route-table)</span></span> |
| <span data-ttu-id="69b63-137">**Konfigurace IP adresy**</span><span class="sxs-lookup"><span data-stu-id="69b63-137">**ipConfigurations**</span></span> |<span data-ttu-id="69b63-138">Kolekce objektů configruation IP používané síťové adaptéry připojené toohello podsítě</span><span class="sxs-lookup"><span data-stu-id="69b63-138">Collection of IP configruation objects used by NICs connected toohello subnet</span></span> |<span data-ttu-id="69b63-139">v tématu [UDR](#Route-table)</span><span class="sxs-lookup"><span data-stu-id="69b63-139">see [UDR](#Route-table)</span></span> |

<span data-ttu-id="69b63-140">Ukázka VNet ve formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="69b63-140">Sample VNet in JSON format:</span></span>

    {
        "name": "TestVNet",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/virtualNetworks",
        "location": "westus",
        "tags": {
            "displayName": "VNet"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "addressSpace": {
                "addressPrefixes": [
                    "192.168.0.0/16"
                ]
            },
            "subnets": [
                {
                    "name": "FrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "networkSecurityGroup": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd"
                        },
                        "routeTable": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                        },
                        "ipConfigurations": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                            },
                            ...]
                    }
                },
                ...]
        }
    }

### <a name="additional-resources"></a><span data-ttu-id="69b63-141">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="69b63-141">Additional resources</span></span>
* <span data-ttu-id="69b63-142">Přečtěte si další informace o [VNet](../articles/virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="69b63-142">Get more information about [VNet](../articles/virtual-network/virtual-networks-overview.md).</span></span>
* <span data-ttu-id="69b63-143">Čtení hello [referenční dokumentace rozhraní API REST](https://msdn.microsoft.com/library/azure/mt163650.aspx) pro virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="69b63-143">Read hello [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163650.aspx) for VNets.</span></span>
* <span data-ttu-id="69b63-144">Čtení hello [referenční dokumentace rozhraní API REST](https://msdn.microsoft.com/library/azure/mt163618.aspx) pro podsítě.</span><span class="sxs-lookup"><span data-stu-id="69b63-144">Read hello [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163618.aspx) for Subnets.</span></span>

