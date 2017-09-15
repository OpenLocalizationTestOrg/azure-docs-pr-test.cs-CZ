## <a name="virtual-network"></a><span data-ttu-id="39a10-101">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="39a10-101">Virtual Network</span></span>
<span data-ttu-id="39a10-102">Virtuální sítí (VNET) a podsítě prostředky pomohl definovat hranici zabezpečení pro úlohy běžící v Azure.</span><span class="sxs-lookup"><span data-stu-id="39a10-102">Virtual Networks (VNET) and subnets resources help define a security boundary for workloads running in Azure.</span></span> <span data-ttu-id="39a10-103">Virtuální síť je charakterizovaná kolekce adresní prostory, které jsou definované jako bloků CIDR.</span><span class="sxs-lookup"><span data-stu-id="39a10-103">A VNet is characterized by a collection of address spaces, defined as CIDR blocks.</span></span> 

> [!NOTE]
> <span data-ttu-id="39a10-104">Správci sítě se seznámíte s notaci CIDR.</span><span class="sxs-lookup"><span data-stu-id="39a10-104">Network administrators are familiar with CIDR notation.</span></span> <span data-ttu-id="39a10-105">Pokud nejste obeznámeni s CIDR, [Další informace o](http://whatismyipaddress.com/cidr).</span><span class="sxs-lookup"><span data-stu-id="39a10-105">If you are not familiar with CIDR, [learn more about it](http://whatismyipaddress.com/cidr).</span></span>
> 
> 

![Virtuální síť s více podsítěmi](./media/resource-groups-networking/Figure4.png)

<span data-ttu-id="39a10-107">Virtuální sítě obsahují následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39a10-107">VNets contain the following properties.</span></span>

| <span data-ttu-id="39a10-108">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="39a10-108">Property</span></span> | <span data-ttu-id="39a10-109">Popis</span><span class="sxs-lookup"><span data-stu-id="39a10-109">Description</span></span> | <span data-ttu-id="39a10-110">Ukázkové hodnoty</span><span class="sxs-lookup"><span data-stu-id="39a10-110">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="39a10-111">**adresní prostor**</span><span class="sxs-lookup"><span data-stu-id="39a10-111">**addressSpace**</span></span> |<span data-ttu-id="39a10-112">Kolekce předpon adres, které tvoří virtuální sítě v notaci CIDR</span><span class="sxs-lookup"><span data-stu-id="39a10-112">Collection of address prefixes that make up the VNet in CIDR notation</span></span> |<span data-ttu-id="39a10-113">192.168.0.0/16</span><span class="sxs-lookup"><span data-stu-id="39a10-113">192.168.0.0/16</span></span> |
| <span data-ttu-id="39a10-114">**podsítě**</span><span class="sxs-lookup"><span data-stu-id="39a10-114">**subnets**</span></span> |<span data-ttu-id="39a10-115">Kolekce podsítě, které tvoří virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="39a10-115">Collection of subnets that make up the VNet</span></span> |<span data-ttu-id="39a10-116">v tématu [podsítě](#Subnets) níže.</span><span class="sxs-lookup"><span data-stu-id="39a10-116">see [subnets](#Subnets) below.</span></span> |
| <span data-ttu-id="39a10-117">**IP adresa**</span><span class="sxs-lookup"><span data-stu-id="39a10-117">**ipAddress**</span></span> |<span data-ttu-id="39a10-118">Přiřazené objektu IP adresy.</span><span class="sxs-lookup"><span data-stu-id="39a10-118">IP address assigned to object.</span></span> <span data-ttu-id="39a10-119">Toto je vlastnost jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="39a10-119">This is a read-only property.</span></span> |<span data-ttu-id="39a10-120">104.42.233.77</span><span class="sxs-lookup"><span data-stu-id="39a10-120">104.42.233.77</span></span> |

### <a name="subnets"></a><span data-ttu-id="39a10-121">Podsítě</span><span class="sxs-lookup"><span data-stu-id="39a10-121">Subnets</span></span>
<span data-ttu-id="39a10-122">Podsíť je prostředkem podřízené virtuální sítě, a pomáhá definovat segmenty adresní prostory v rámci blok CIDR pomocí předpony IP adres.</span><span class="sxs-lookup"><span data-stu-id="39a10-122">A subnet is a child resource of a VNet, and helps define segments of address spaces within a CIDR block, using IP address prefixes.</span></span> <span data-ttu-id="39a10-123">Síťové adaptéry můžete přidat do podsítí a připojení k virtuálním počítačům, poskytuje připojení pro různé úlohy.</span><span class="sxs-lookup"><span data-stu-id="39a10-123">NICs can be added to subnets, and connected to VMs, providing connectivity for various workloads.</span></span>

<span data-ttu-id="39a10-124">Podsítě obsahují následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39a10-124">Subnets contain the following properties.</span></span> 

| <span data-ttu-id="39a10-125">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="39a10-125">Property</span></span> | <span data-ttu-id="39a10-126">Popis</span><span class="sxs-lookup"><span data-stu-id="39a10-126">Description</span></span> | <span data-ttu-id="39a10-127">Ukázkové hodnoty</span><span class="sxs-lookup"><span data-stu-id="39a10-127">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="39a10-128">**addressPrefix**</span><span class="sxs-lookup"><span data-stu-id="39a10-128">**addressPrefix**</span></span> |<span data-ttu-id="39a10-129">Jedna adresa předponu, která tvoří podsíť v notaci CIDR</span><span class="sxs-lookup"><span data-stu-id="39a10-129">Single address prefix that make up the subnet in CIDR notation</span></span> |<span data-ttu-id="39a10-130">192.168.1.0/24</span><span class="sxs-lookup"><span data-stu-id="39a10-130">192.168.1.0/24</span></span> |
| <span data-ttu-id="39a10-131">**skupinu zabezpečení sítě**</span><span class="sxs-lookup"><span data-stu-id="39a10-131">**networkSecurityGroup**</span></span> |<span data-ttu-id="39a10-132">Skupina NSG použije na podsíť</span><span class="sxs-lookup"><span data-stu-id="39a10-132">NSG applied to the subnet</span></span> |<span data-ttu-id="39a10-133">v tématu [skupiny Nsg](#Network-Security-Group)</span><span class="sxs-lookup"><span data-stu-id="39a10-133">see [NSGs](#Network-Security-Group)</span></span> |
| <span data-ttu-id="39a10-134">**routeTable**</span><span class="sxs-lookup"><span data-stu-id="39a10-134">**routeTable**</span></span> |<span data-ttu-id="39a10-135">Směrovací tabulka použije na podsíť</span><span class="sxs-lookup"><span data-stu-id="39a10-135">Route table applied to the subnet</span></span> |<span data-ttu-id="39a10-136">v tématu [UDR](#Route-table)</span><span class="sxs-lookup"><span data-stu-id="39a10-136">see [UDR](#Route-table)</span></span> |
| <span data-ttu-id="39a10-137">**Konfigurace IP adresy**</span><span class="sxs-lookup"><span data-stu-id="39a10-137">**ipConfigurations**</span></span> |<span data-ttu-id="39a10-138">Kolekce objektů configruation IP používané síťové adaptéry připojené k podsíti</span><span class="sxs-lookup"><span data-stu-id="39a10-138">Collection of IP configruation objects used by NICs connected to the subnet</span></span> |<span data-ttu-id="39a10-139">v tématu [UDR](#Route-table)</span><span class="sxs-lookup"><span data-stu-id="39a10-139">see [UDR](#Route-table)</span></span> |

<span data-ttu-id="39a10-140">Ukázka VNet ve formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="39a10-140">Sample VNet in JSON format:</span></span>

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

### <a name="additional-resources"></a><span data-ttu-id="39a10-141">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="39a10-141">Additional resources</span></span>
* <span data-ttu-id="39a10-142">Přečtěte si další informace o [VNet](../articles/virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="39a10-142">Get more information about [VNet](../articles/virtual-network/virtual-networks-overview.md).</span></span>
* <span data-ttu-id="39a10-143">Pro čtení [referenční dokumentace rozhraní API REST](https://msdn.microsoft.com/library/azure/mt163650.aspx) pro virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="39a10-143">Read the [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163650.aspx) for VNets.</span></span>
* <span data-ttu-id="39a10-144">Pro čtení [referenční dokumentace rozhraní API REST](https://msdn.microsoft.com/library/azure/mt163618.aspx) pro podsítě.</span><span class="sxs-lookup"><span data-stu-id="39a10-144">Read the [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163618.aspx) for Subnets.</span></span>

