## <a name="network-security-group"></a><span data-ttu-id="2fc6c-101">Skupina zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="2fc6c-101">Network Security Group</span></span>
<span data-ttu-id="2fc6c-102">Prostředek NSG umožňuje hello vytvoření hranice zabezpečení pro úlohy, implementací povolit a zakázat pravidla.</span><span class="sxs-lookup"><span data-stu-id="2fc6c-102">An NSG resource enables hello creation of security boundary for workloads, by implementing allow and deny rules.</span></span> <span data-ttu-id="2fc6c-103">Tato pravidla lze použít tooa virtuálních počítačů, síťové karty nebo podsíť.</span><span class="sxs-lookup"><span data-stu-id="2fc6c-103">Such rules can be applied tooa VM, a NIC, or a subnet.</span></span>

| <span data-ttu-id="2fc6c-104">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="2fc6c-104">Property</span></span> | <span data-ttu-id="2fc6c-105">Popis</span><span class="sxs-lookup"><span data-stu-id="2fc6c-105">Description</span></span> | <span data-ttu-id="2fc6c-106">Ukázkové hodnoty</span><span class="sxs-lookup"><span data-stu-id="2fc6c-106">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2fc6c-107">**podsítě**</span><span class="sxs-lookup"><span data-stu-id="2fc6c-107">**subnets**</span></span> |<span data-ttu-id="2fc6c-108">Seznam hello ID podsítě NSG se použije pro.</span><span class="sxs-lookup"><span data-stu-id="2fc6c-108">List of subnet ids hello NSG is applied to.</span></span> |<span data-ttu-id="2fc6c-109">/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd</span><span class="sxs-lookup"><span data-stu-id="2fc6c-109">/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd</span></span> |
| <span data-ttu-id="2fc6c-110">**securityRules**</span><span class="sxs-lookup"><span data-stu-id="2fc6c-110">**securityRules**</span></span> |<span data-ttu-id="2fc6c-111">Seznam pravidel zabezpečení, které tvoří hello NSG</span><span class="sxs-lookup"><span data-stu-id="2fc6c-111">List of security rules that make up hello NSG</span></span> |<span data-ttu-id="2fc6c-112">V tématu [pravidlo zabezpečení](#Security-rule) níže</span><span class="sxs-lookup"><span data-stu-id="2fc6c-112">See [Security rule](#Security-rule) below</span></span> |
| <span data-ttu-id="2fc6c-113">**defaultSecurityRules**</span><span class="sxs-lookup"><span data-stu-id="2fc6c-113">**defaultSecurityRules**</span></span> |<span data-ttu-id="2fc6c-114">Seznam výchozích pravidel zabezpečení, které jsou v každé skupiny NSG</span><span class="sxs-lookup"><span data-stu-id="2fc6c-114">List of default security rules present in every NSG</span></span> |<span data-ttu-id="2fc6c-115">V tématu [výchozí pravidla zabezpečení](#Default-security-rules) níže</span><span class="sxs-lookup"><span data-stu-id="2fc6c-115">See [Default security rules](#Default-security-rules) below</span></span> |

* <span data-ttu-id="2fc6c-116">**Pravidlo zabezpečení** -NSG můžete mít je definováno více pravidel zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="2fc6c-116">**Security rule** - An NSG can have multiple security rules defined.</span></span> <span data-ttu-id="2fc6c-117">Každé pravidlo můžete povolit nebo odepřít různé typy provozu.</span><span class="sxs-lookup"><span data-stu-id="2fc6c-117">Each rule can allow or deny different types of traffic.</span></span>

### <a name="security-rule"></a><span data-ttu-id="2fc6c-118">Pravidlo zabezpečení</span><span class="sxs-lookup"><span data-stu-id="2fc6c-118">Security rule</span></span>
<span data-ttu-id="2fc6c-119">Pravidlo zabezpečení je prostředkem podřízenou skupinu NSG obsahující vlastnosti hello níže.</span><span class="sxs-lookup"><span data-stu-id="2fc6c-119">A security rule is a child resource of an NSG containing hello properties below.</span></span>

| <span data-ttu-id="2fc6c-120">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="2fc6c-120">Property</span></span> | <span data-ttu-id="2fc6c-121">Popis</span><span class="sxs-lookup"><span data-stu-id="2fc6c-121">Description</span></span> | <span data-ttu-id="2fc6c-122">Ukázkové hodnoty</span><span class="sxs-lookup"><span data-stu-id="2fc6c-122">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2fc6c-123">**Popis**</span><span class="sxs-lookup"><span data-stu-id="2fc6c-123">**description**</span></span> |<span data-ttu-id="2fc6c-124">Popis pravidla hello</span><span class="sxs-lookup"><span data-stu-id="2fc6c-124">Description for hello rule</span></span> |<span data-ttu-id="2fc6c-125">Povolí příchozí komunikaci pro všechny virtuální počítače v podsíti X</span><span class="sxs-lookup"><span data-stu-id="2fc6c-125">Allow inbound traffic for all VMs in subnet X</span></span> |
| <span data-ttu-id="2fc6c-126">**protokol**</span><span class="sxs-lookup"><span data-stu-id="2fc6c-126">**protocol**</span></span> |<span data-ttu-id="2fc6c-127">Protokol toomatch pro pravidlo hello</span><span class="sxs-lookup"><span data-stu-id="2fc6c-127">Protocol toomatch for hello rule</span></span> |<span data-ttu-id="2fc6c-128">TCP, UDP nebo *</span><span class="sxs-lookup"><span data-stu-id="2fc6c-128">TCP, UDP, or *</span></span> |
| <span data-ttu-id="2fc6c-129">**sourcePortRange**</span><span class="sxs-lookup"><span data-stu-id="2fc6c-129">**sourcePortRange**</span></span> |<span data-ttu-id="2fc6c-130">Zdrojový port rozsah toomatch pro pravidlo hello</span><span class="sxs-lookup"><span data-stu-id="2fc6c-130">Source port range toomatch for hello rule</span></span> |<span data-ttu-id="2fc6c-131">80, 100-200, *</span><span class="sxs-lookup"><span data-stu-id="2fc6c-131">80, 100-200, *</span></span> |
| <span data-ttu-id="2fc6c-132">**destinationPortRange**</span><span class="sxs-lookup"><span data-stu-id="2fc6c-132">**destinationPortRange**</span></span> |<span data-ttu-id="2fc6c-133">Cílový port rozsah toomatch pro pravidlo hello</span><span class="sxs-lookup"><span data-stu-id="2fc6c-133">Destination port range toomatch for hello rule</span></span> |<span data-ttu-id="2fc6c-134">80, 100-200, *</span><span class="sxs-lookup"><span data-stu-id="2fc6c-134">80, 100-200, *</span></span> |
| <span data-ttu-id="2fc6c-135">**sourceAddressPrefix**</span><span class="sxs-lookup"><span data-stu-id="2fc6c-135">**sourceAddressPrefix**</span></span> |<span data-ttu-id="2fc6c-136">Toomatch předpona zdrojové adresy pro pravidlo hello</span><span class="sxs-lookup"><span data-stu-id="2fc6c-136">Source address prefix toomatch for hello rule</span></span> |<span data-ttu-id="2fc6c-137">10.10.10.1 10.10.10.0/24, virtuální síť</span><span class="sxs-lookup"><span data-stu-id="2fc6c-137">10.10.10.1, 10.10.10.0/24, VirtualNetwork</span></span> |
| <span data-ttu-id="2fc6c-138">**destinationAddressPrefix**</span><span class="sxs-lookup"><span data-stu-id="2fc6c-138">**destinationAddressPrefix**</span></span> |<span data-ttu-id="2fc6c-139">Toomatch předpona cílové adresy pro pravidlo hello</span><span class="sxs-lookup"><span data-stu-id="2fc6c-139">Destination address prefix toomatch for hello rule</span></span> |<span data-ttu-id="2fc6c-140">10.10.10.1 10.10.10.0/24, virtuální síť</span><span class="sxs-lookup"><span data-stu-id="2fc6c-140">10.10.10.1, 10.10.10.0/24, VirtualNetwork</span></span> |
| <span data-ttu-id="2fc6c-141">**směr**</span><span class="sxs-lookup"><span data-stu-id="2fc6c-141">**direction**</span></span> |<span data-ttu-id="2fc6c-142">Směr provozu toomatch pro pravidlo hello</span><span class="sxs-lookup"><span data-stu-id="2fc6c-142">Direction of traffic toomatch for hello rule</span></span> |<span data-ttu-id="2fc6c-143">Příchozí nebo odchozí</span><span class="sxs-lookup"><span data-stu-id="2fc6c-143">inbound or outbound</span></span> |
| <span data-ttu-id="2fc6c-144">**Priorita**</span><span class="sxs-lookup"><span data-stu-id="2fc6c-144">**priority**</span></span> |<span data-ttu-id="2fc6c-145">Priorita pravidla hello.</span><span class="sxs-lookup"><span data-stu-id="2fc6c-145">Priority for hello rule.</span></span> <span data-ttu-id="2fc6c-146">Pravidla se kontrolují v pořadí podle priority, jakmile se pravidlo vztahuje, žádná další pravidla se již nekontrolují.</span><span class="sxs-lookup"><span data-stu-id="2fc6c-146">Rules are checked int he order of priority, once a rule applies, no more rules are tested for matching.</span></span> |<span data-ttu-id="2fc6c-147">10, 100, 65000</span><span class="sxs-lookup"><span data-stu-id="2fc6c-147">10, 100, 65000</span></span> |
| <span data-ttu-id="2fc6c-148">**přístup**</span><span class="sxs-lookup"><span data-stu-id="2fc6c-148">**access**</span></span> |<span data-ttu-id="2fc6c-149">Typ tooapply přístup, pokud odpovídá pravidlo hello</span><span class="sxs-lookup"><span data-stu-id="2fc6c-149">Type of access tooapply if hello rule matches</span></span> |<span data-ttu-id="2fc6c-150">Povolit nebo odepřít</span><span class="sxs-lookup"><span data-stu-id="2fc6c-150">allow or deny</span></span> |

<span data-ttu-id="2fc6c-151">Ukázka NSG ve formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="2fc6c-151">Sample NSG in JSON format:</span></span>

    {
        "name": "NSG-BackEnd",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/networkSecurityGroups",
        "location": "westus",
        "tags": {
            "displayName": "NSG - Front End"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "securityRules": [
                {
                    "name": "rdp-rule",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd/securityRules/rdp-rule",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "description": "Allow RDP",
                        "protocol": "Tcp",
                        "sourcePortRange": "*",
                        "destinationPortRange": "3389",
                        "sourceAddressPrefix": "Internet",
                        "destinationAddressPrefix": "*",
                        "access": "Allow",
                        "priority": 100,
                        "direction": "Inbound"
                    }
                }
            ],
            "defaultSecurityRules": [
                { [...],
            "subnets": [
                {
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
                }
            ]
        }
    }

### <a name="default-security-rules"></a><span data-ttu-id="2fc6c-152">Výchozí pravidla zabezpečení</span><span class="sxs-lookup"><span data-stu-id="2fc6c-152">Default security rules</span></span>

<span data-ttu-id="2fc6c-153">Výchozí pravidla zabezpečení mají hello stejné vlastnosti, které jsou k dispozici v pravidla zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="2fc6c-153">Default security rules have hello same properties available in security rules.</span></span> <span data-ttu-id="2fc6c-154">Existují tooprovide základní připojení mezi prostředky, které mají toothem použít skupiny Nsg.</span><span class="sxs-lookup"><span data-stu-id="2fc6c-154">They exist tooprovide basic connectivity between resources that have NSGs applied toothem.</span></span> <span data-ttu-id="2fc6c-155">Ujistěte se, které znáte [výchozí pravidla zabezpečení](../articles/virtual-network/virtual-networks-nsg.md#default-rules) neexistuje.</span><span class="sxs-lookup"><span data-stu-id="2fc6c-155">Make sure you know which [default security rules](../articles/virtual-network/virtual-networks-nsg.md#default-rules) exist.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="2fc6c-156">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2fc6c-156">Additional resources</span></span>
* <span data-ttu-id="2fc6c-157">Přečtěte si další informace o [skupiny Nsg](../articles/virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="2fc6c-157">Get more information about [NSGs](../articles/virtual-network/virtual-networks-nsg.md).</span></span>
* <span data-ttu-id="2fc6c-158">Čtení hello [referenční dokumentace rozhraní API REST](https://msdn.microsoft.com/library/azure/mt163615.aspx) pro skupiny Nsg.</span><span class="sxs-lookup"><span data-stu-id="2fc6c-158">Read hello [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163615.aspx) for NSGs.</span></span>
* <span data-ttu-id="2fc6c-159">Čtení hello [referenční dokumentace rozhraní API REST](https://msdn.microsoft.com/library/azure/mt163580.aspx) pro pravidla zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="2fc6c-159">Read hello [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163580.aspx) for security rules.</span></span>
