## <a name="nic"></a><span data-ttu-id="30265-101">SÍŤOVÝ ADAPTÉR</span><span class="sxs-lookup"><span data-stu-id="30265-101">NIC</span></span>
<span data-ttu-id="30265-102">Karta (NIC) prostředku síťového rozhraní poskytuje síťové připojení tooan existující podsíť v prostředku virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="30265-102">A network interface card (NIC) resource provides network connectivity tooan existing subnet in a VNet resource.</span></span> <span data-ttu-id="30265-103">I když můžete vytvořit síťový adaptér jako samostatný objekt, je nutné tooassociate ho tooanother objekt tooactually poskytovat připojení.</span><span class="sxs-lookup"><span data-stu-id="30265-103">Although you can create a NIC as a stand alone object, you need tooassociate it tooanother object tooactually provide connectivity.</span></span> <span data-ttu-id="30265-104">Síťový adaptér může být použité tooconnect tooa podsíť virtuálních počítačů, veřejnou IP adresu nebo Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="30265-104">A NIC can be used tooconnect a VM tooa subnet, a public IP address, or a load balancer.</span></span>  

| <span data-ttu-id="30265-105">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="30265-105">Property</span></span> | <span data-ttu-id="30265-106">Popis</span><span class="sxs-lookup"><span data-stu-id="30265-106">Description</span></span> | <span data-ttu-id="30265-107">Ukázkové hodnoty</span><span class="sxs-lookup"><span data-stu-id="30265-107">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="30265-108">**virtuální počítač**</span><span class="sxs-lookup"><span data-stu-id="30265-108">**virtualMachine**</span></span> |<span data-ttu-id="30265-109">Virtuální počítač hello síťový adaptér je přidružen.</span><span class="sxs-lookup"><span data-stu-id="30265-109">VM hello NIC is associated with.</span></span> |<span data-ttu-id="30265-110">/subscriptions/{GUID}/../microsoft.COMPUTE/virtualMachines/vm1</span><span class="sxs-lookup"><span data-stu-id="30265-110">/subscriptions/{guid}/../Microsoft.Compute/virtualMachines/vm1</span></span> |
| <span data-ttu-id="30265-111">**macAddress**</span><span class="sxs-lookup"><span data-stu-id="30265-111">**macAddress**</span></span> |<span data-ttu-id="30265-112">Adresa MAC pro síťový adaptér hello</span><span class="sxs-lookup"><span data-stu-id="30265-112">MAC address for hello NIC</span></span> |<span data-ttu-id="30265-113">Libovolná hodnota od 4 do 30.</span><span class="sxs-lookup"><span data-stu-id="30265-113">any value between 4 and 30</span></span> |
| <span data-ttu-id="30265-114">**skupinu zabezpečení sítě**</span><span class="sxs-lookup"><span data-stu-id="30265-114">**networkSecurityGroup**</span></span> |<span data-ttu-id="30265-115">Toohello NSG přidruženou síťovou kartu</span><span class="sxs-lookup"><span data-stu-id="30265-115">NSG associated toohello NIC</span></span> |<span data-ttu-id="30265-116">/subscriptions/{GUID}/../microsoft.Network/networkSecurityGroups/myNSG1</span><span class="sxs-lookup"><span data-stu-id="30265-116">/subscriptions/{guid}/../Microsoft.Network/networkSecurityGroups/myNSG1</span></span> |
| <span data-ttu-id="30265-117">**dnsSettings**</span><span class="sxs-lookup"><span data-stu-id="30265-117">**dnsSettings**</span></span> |<span data-ttu-id="30265-118">Nastavení DNS pro hello síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="30265-118">DNS settings for hello NIC</span></span> |<span data-ttu-id="30265-119">v tématu [PIP](#Public-IP-address)</span><span class="sxs-lookup"><span data-stu-id="30265-119">see [PIP](#Public-IP-address)</span></span> |

<span data-ttu-id="30265-120">Síťový adaptér nebo síťový adaptér představuje síťové rozhraní, které může být přidružené tooa virtuální počítač (VM).</span><span class="sxs-lookup"><span data-stu-id="30265-120">A Network Interface Card, or NIC, represents a network interface that can be associated tooa virtual machine (VM).</span></span> <span data-ttu-id="30265-121">Virtuální počítač může mít jeden nebo více síťových adaptérů.</span><span class="sxs-lookup"><span data-stu-id="30265-121">A VM can have one or more NICs.</span></span>

![Karty síťového rozhraní na jeden virtuální počítač](./media/resource-groups-networking/Figure3.png)

### <a name="ip-configurations"></a><span data-ttu-id="30265-123">Konfigurace protokolu IP</span><span class="sxs-lookup"><span data-stu-id="30265-123">IP configurations</span></span>
<span data-ttu-id="30265-124">Síťové adaptéry mít podřízený objekt s názvem **ipConfigurations** obsahující hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="30265-124">NICs have a child object named **ipConfigurations** containing hello following properties:</span></span>

| <span data-ttu-id="30265-125">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="30265-125">Property</span></span> | <span data-ttu-id="30265-126">Popis</span><span class="sxs-lookup"><span data-stu-id="30265-126">Description</span></span> | <span data-ttu-id="30265-127">Ukázkové hodnoty</span><span class="sxs-lookup"><span data-stu-id="30265-127">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="30265-128">**podsíť**</span><span class="sxs-lookup"><span data-stu-id="30265-128">**subnet**</span></span> |<span data-ttu-id="30265-129">Podsíť hello síťový adaptér je onnected k.</span><span class="sxs-lookup"><span data-stu-id="30265-129">Subnet hello NIC is onnected to.</span></span> |<span data-ttu-id="30265-130">/subscriptions/{GUID}/../microsoft.Network/virtualNetworks/myvnet1/subnets/mysub1</span><span class="sxs-lookup"><span data-stu-id="30265-130">/subscriptions/{guid}/../Microsoft.Network/virtualNetworks/myvnet1/subnets/mysub1</span></span> |
| <span data-ttu-id="30265-131">**privateIPAddress**</span><span class="sxs-lookup"><span data-stu-id="30265-131">**privateIPAddress**</span></span> |<span data-ttu-id="30265-132">IP adresa pro hello síťový adaptér v podsíti hello</span><span class="sxs-lookup"><span data-stu-id="30265-132">IP address for hello NIC in hello subnet</span></span> |<span data-ttu-id="30265-133">10.0.0.8</span><span class="sxs-lookup"><span data-stu-id="30265-133">10.0.0.8</span></span> |
| <span data-ttu-id="30265-134">**privateIPAllocationMethod**</span><span class="sxs-lookup"><span data-stu-id="30265-134">**privateIPAllocationMethod**</span></span> |<span data-ttu-id="30265-135">Metoda přidělení IP</span><span class="sxs-lookup"><span data-stu-id="30265-135">IP allocation method</span></span> |<span data-ttu-id="30265-136">Dynamická nebo statická</span><span class="sxs-lookup"><span data-stu-id="30265-136">Dynamic or Static</span></span> |
| <span data-ttu-id="30265-137">**enableIPForwarding**</span><span class="sxs-lookup"><span data-stu-id="30265-137">**enableIPForwarding**</span></span> |<span data-ttu-id="30265-138">Jestli se dá použít hello síťový adaptér pro směrování</span><span class="sxs-lookup"><span data-stu-id="30265-138">Whether hello NIC can be used for routing</span></span> |<span data-ttu-id="30265-139">hodnotu true nebo false</span><span class="sxs-lookup"><span data-stu-id="30265-139">true or false</span></span> |
| <span data-ttu-id="30265-140">**primární**</span><span class="sxs-lookup"><span data-stu-id="30265-140">**primary**</span></span> |<span data-ttu-id="30265-141">Jestli je hello síťový adaptér hello primární síťový adaptér pro hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="30265-141">Whether hello NIC is hello primary NIC for hello VM</span></span> |<span data-ttu-id="30265-142">hodnotu true nebo false</span><span class="sxs-lookup"><span data-stu-id="30265-142">true or false</span></span> |
| <span data-ttu-id="30265-143">**publicIPAddress**</span><span class="sxs-lookup"><span data-stu-id="30265-143">**publicIPAddress**</span></span> |<span data-ttu-id="30265-144">PIP přidružené hello síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="30265-144">PIP associated with hello NIC</span></span> |<span data-ttu-id="30265-145">v tématu [nastavení DNS](#DNS-settings)</span><span class="sxs-lookup"><span data-stu-id="30265-145">see [DNS Settings](#DNS-settings)</span></span> |
| <span data-ttu-id="30265-146">**pravidlo loadBalancerBackendAddressPools**</span><span class="sxs-lookup"><span data-stu-id="30265-146">**loadBalancerBackendAddressPools**</span></span> |<span data-ttu-id="30265-147">Back end adresu fondy hello síťový adaptér je spojené s</span><span class="sxs-lookup"><span data-stu-id="30265-147">Back end address pools hello NIC is associated with</span></span> | |
| <span data-ttu-id="30265-148">**loadBalancerInboundNatRules**</span><span class="sxs-lookup"><span data-stu-id="30265-148">**loadBalancerInboundNatRules**</span></span> |<span data-ttu-id="30265-149">Příchozí zatížení vyrovnávání NAT pravidla hello síťový adaptér je spojené s</span><span class="sxs-lookup"><span data-stu-id="30265-149">Inbound load balancer NAT rules hello NIC is associated with</span></span> | |

<span data-ttu-id="30265-150">Ukázka veřejnou IP adresu ve formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="30265-150">Sample public IP address in JSON format:</span></span>

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

### <a name="additional-resources"></a><span data-ttu-id="30265-151">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="30265-151">Additional resources</span></span>
* <span data-ttu-id="30265-152">Čtení hello [referenční dokumentace rozhraní API REST](https://msdn.microsoft.com/library/azure/mt163579.aspx) pro síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="30265-152">Read hello [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163579.aspx) for NICs.</span></span>

