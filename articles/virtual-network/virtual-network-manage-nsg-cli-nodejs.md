---
title: "Správa skupin zabezpečení sítě - 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Naučte se spravovat skupiny zabezpečení sítě pomocí rozhraní příkazového řádku Azure (CLI) 1.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2e53c3ff2ffbef95d6b72ca6afb3b4de377f0389
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-network-security-groups-using-the-azure-cli-10"></a><span data-ttu-id="fe116-103">Správa skupin zabezpečení sítě pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="fe116-103">Manage network security groups using the Azure CLI 1.0</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="fe116-104">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="fe116-104">CLI versions to complete the task</span></span> 

<span data-ttu-id="fe116-105">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="fe116-105">You can complete the task using one of the following CLI versions:</span></span> 

- <span data-ttu-id="fe116-106">[Azure CLI 1.0](#View-existing-NSGs) – naše rozhraní příkazového řádku pro klasické modely nasazení a modely nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="fe116-106">[Azure CLI 1.0](#View-existing-NSGs) – our CLI for the classic and resource management deployment models</span></span> 
- <span data-ttu-id="fe116-107">[Azure CLI 2.0](virtual-network-manage-nsg-arm-cli.md) -naší nové generace rozhraní příkazového řádku pro správu model nasazení prostředku (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="fe116-107">[Azure CLI 2.0](virtual-network-manage-nsg-arm-cli.md) - our next generation CLI for the resource management deployment model (this article)</span></span>

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="fe116-108">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="fe116-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="fe116-109">Tento článek se zabývá pomocí modelu nasazení Resource Manager, které společnost Microsoft doporučuje pro většinu nových nasazení místo modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="fe116-109">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the classic deployment model.</span></span>
> 

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="retrieve-information"></a><span data-ttu-id="fe116-110">Načtení informací o</span><span class="sxs-lookup"><span data-stu-id="fe116-110">Retrieve Information</span></span>
<span data-ttu-id="fe116-111">Můžete zobrazit stávající skupiny Nsg, načíst pravidla pro existující skupiny NSG a zjistit, jaké prostředky skupinu NSG je přidružena k.</span><span class="sxs-lookup"><span data-stu-id="fe116-111">You can view your existing NSGs, retrieve rules for an existing NSG, and find out what resources an NSG is associated to.</span></span>

### <a name="view-existing-nsgs"></a><span data-ttu-id="fe116-112">Zobrazit existující skupiny Nsg</span><span class="sxs-lookup"><span data-stu-id="fe116-112">View existing NSGs</span></span>
<span data-ttu-id="fe116-113">Chcete-li zobrazit seznam skupin Nsg v určité skupiny zdrojů, spusťte `azure network nsg list` příkaz, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="fe116-113">To view the list of NSGs in a specific resource group, run the `azure network nsg list` command as shown below.</span></span>

```azurecli
azure network nsg list --resource-group RG-NSG
```

<span data-ttu-id="fe116-114">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="fe116-114">Expected output:</span></span>

    info:    Executing command network nsg list
    + Getting the network security groups
    data:    Name          Location
    data:    ------------  --------
    data:    NSG-BackEnd   westus
    data:    NSG-FrontEnd  westus
    info:    network nsg list command OK

### <a name="list-all-rules-for-an-nsg"></a><span data-ttu-id="fe116-115">Seznam všech pravidel pro skupiny NSG</span><span class="sxs-lookup"><span data-stu-id="fe116-115">List all rules for an NSG</span></span>
<span data-ttu-id="fe116-116">Chcete-li zobrazit pravidla s názvem skupiny NSG **NSG front-endu**spusťte `azure network nsg show` příkaz, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="fe116-116">To view the rules of an NSG named **NSG-FrontEnd**, run the `azure network nsg show` command as shown below.</span></span> 

```azurecli
azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd
```

<span data-ttu-id="fe116-117">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="fe116-117">Expected output:</span></span>

    info:    Executing command network nsg show
    + Looking up the network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    data:    Name                            : NSG-FrontEnd
    data:    Type                            : Microsoft.Network/networkSecurityGroups
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    Tags                            : displayName=NSG - Front End
    data:    Security group rules:
    data:    Name                           Source IP          Source Port  Destination IP  Destination Port  Protocol  Direction  Access  Priority
    data:    -----------------------------  -----------------  -----------  --------------  ----------------  --------  ---------  ------  --------
    data:    rdp-rule                       Internet           *            *               3389              Tcp       Inbound    Allow   100
    data:    web-rule                       Internet           *            *               80                Tcp       Inbound    Allow   101
    data:    AllowVnetInBound               VirtualNetwork     *            VirtualNetwork  *                 *         Inbound    Allow   65000
    data:    AllowAzureLoadBalancerInBound  AzureLoadBalancer  *            *               *                 *         Inbound    Allow   65001
    data:    DenyAllInBound                 *                  *            *               *                 *         Inbound    Deny    65500
    data:    AllowVnetOutBound              VirtualNetwork     *            VirtualNetwork  *                 *         Outbound   Allow   65000
    data:    AllowInternetOutBound          *                  *            Internet        *                 *         Outbound   Allow   65001
    data:    DenyAllOutBound                *                  *            *               *                 *         Outbound   Deny    65500
    info:    network nsg show command OK

> [!NOTE]
> <span data-ttu-id="fe116-118">Můžete také použít `azure network nsg rule list --resource-group RG-NSG --nsg-name NSG-FrontEnd` seznam pravidel z **NSG front-endu** NSG.</span><span class="sxs-lookup"><span data-stu-id="fe116-118">You can also use `azure network nsg rule list --resource-group RG-NSG --nsg-name NSG-FrontEnd` to list the rules from the **NSG-FrontEnd** NSG.</span></span>
>

### <a name="view-nsg-associations"></a><span data-ttu-id="fe116-119">Zobrazit přidružení skupiny NSG</span><span class="sxs-lookup"><span data-stu-id="fe116-119">View NSG associations</span></span>

<span data-ttu-id="fe116-120">Chcete-li zobrazit prostředky **NSG front-endu** NSG je spojený s spustit `azure network nsg show` příkaz, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="fe116-120">To view what resources the **NSG-FrontEnd** NSG is associate with, run the `azure network nsg show` command as shown below.</span></span> <span data-ttu-id="fe116-121">Všimněte si, že je jediným rozdílem je použití **– json** parametr.</span><span class="sxs-lookup"><span data-stu-id="fe116-121">Notice that the only difference is the use of the **--json** parameter.</span></span>

```azurecli
azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd --json
```

<span data-ttu-id="fe116-122">Vyhledejte **networkInterfaces** a **podsítě** vlastnosti, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="fe116-122">Look for the **networkInterfaces** and **subnets** properties as shown below:</span></span>

    "networkInterfaces": [],
    ...
    "subnets": [
        {
            "id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
        }
    ],
    ...

<span data-ttu-id="fe116-123">V předchozím příkladu NSG není přidružen k žádné síťových rozhraní (NIC) a je přidružen k podsíti s názvem **front-endu**.</span><span class="sxs-lookup"><span data-stu-id="fe116-123">In the example above, the NSG is not associated to any network interfaces (NICs), and it is associated to a subnet named **FrontEnd**.</span></span>

## <a name="manage-rules"></a><span data-ttu-id="fe116-124">Spravovat pravidla</span><span class="sxs-lookup"><span data-stu-id="fe116-124">Manage rules</span></span>
<span data-ttu-id="fe116-125">Můžete přidat pravidla do existující skupiny NSG, upravit stávající pravidla a odstranit pravidla.</span><span class="sxs-lookup"><span data-stu-id="fe116-125">You can add rules to an existing NSG, edit existing rules, and remove rules.</span></span>

### <a name="add-a-rule"></a><span data-ttu-id="fe116-126">Přidání pravidla</span><span class="sxs-lookup"><span data-stu-id="fe116-126">Add a rule</span></span>
<span data-ttu-id="fe116-127">Chcete-li přidat pravidlo, které povoluje **příchozí** přenosy na portu **443** z libovolného počítače k **NSG front-endu** NSG, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fe116-127">To add a rule allowing **inbound** traffic to port **443** from any machine to the **NSG-FrontEnd** NSG, enter the following command:</span></span>

```azurecli
azure network nsg rule create --resource-group RG-NSG \
    --nsg-name NSG-FrontEnd \
    --name allow-https \
    --description "Allow access to port 443 for HTTPS" \
    --protocol Tcp \
    --source-address-prefix * \
    --source-port-range * \
    --destination-address-prefix * \
    --destination-port-range 443 \
    --access Allow \
    --priority 102 \
    --direction Inbound
```

<span data-ttu-id="fe116-128">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="fe116-128">Expected output:</span></span>

    info:    Executing command network nsg rule create
    + Looking up the network security rule "allow-https"
    + Creating a network security rule "allow-https"
    + Looking up the network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https
    data:    Name                            : allow-https
    data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
    data:    Provisioning state              : Succeeded
    data:    Description                     : Allow access to port 443 for HTTPS
    data:    Source IP                       : *
    data:    Source Port                     : *
    data:    Destination IP                  : *
    data:    Destination Port                : 443
    data:    Protocol                        : Tcp
    data:    Direction                       : Inbound
    data:    Access                          : Allow
    data:    Priority                        : 102
    info:    network nsg rule create command OK

### <a name="change-a-rule"></a><span data-ttu-id="fe116-129">Změna pravidla</span><span class="sxs-lookup"><span data-stu-id="fe116-129">Change a rule</span></span>
<span data-ttu-id="fe116-130">Chcete-li změnit pravidlo vytvořili výše, které pokud chcete povolit příchozí přenosy z **Internet** pouze, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fe116-130">To change the rule created above to allow inbound traffic from the **Internet** only, run the following command:</span></span>

```azurecli
azure network nsg rule set --resource-group RG-NSG \
    --nsg-name NSG-FrontEnd \
    --name allow-https \
    --source-address-prefix Internet
```

<span data-ttu-id="fe116-131">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="fe116-131">Expected output:</span></span>

    info:    Executing command network nsg rule set
    + Looking up the network security group "NSG-FrontEnd"
    + Setting a network security rule "allow-https"
    + Looking up the network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https
    data:    Name                            : allow-https
    data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
    data:    Provisioning state              : Succeeded
    data:    Description                     : Allow access to port 443 for HTTPS
    data:    Source IP                       : Internet
    data:    Source Port                     : *
    data:    Destination IP                  : *
    data:    Destination Port                : 443
    data:    Protocol                        : Tcp
    data:    Direction                       : Inbound
    data:    Access                          : Allow
    data:    Priority                        : 102
    info:    network nsg rule set command OK

### <a name="delete-a-rule"></a><span data-ttu-id="fe116-132">Odstranění pravidla</span><span class="sxs-lookup"><span data-stu-id="fe116-132">Delete a rule</span></span>
<span data-ttu-id="fe116-133">Pokud chcete odstranit pravidlo vytvořili výše, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fe116-133">To delete the rule created above, run the following command:</span></span>

```azurecli
azure network nsg rule delete --resource-group RG-NSG \
    --nsg-name NSG-FrontEnd \
    --name allow-https \
    --quiet
```

> [!NOTE]
> <span data-ttu-id="fe116-134">`--quiet` Parametr zajišťuje nemusíte potvrďte odstranění.</span><span class="sxs-lookup"><span data-stu-id="fe116-134">The `--quiet` parameter ensures you don't need to confirm the deletion.</span></span>
>

<span data-ttu-id="fe116-135">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="fe116-135">Expected output:</span></span>

    info:    Executing command network nsg rule delete
    + Looking up the network security group "NSG-FrontEnd"
    + Deleting network security rule "allow-https"
    info:    network nsg rule delete command OK

## <a name="manage-associations"></a><span data-ttu-id="fe116-136">Správa přidružení</span><span class="sxs-lookup"><span data-stu-id="fe116-136">Manage associations</span></span>
<span data-ttu-id="fe116-137">Můžete přidružit skupiny NSG k podsítí a síťových karet.</span><span class="sxs-lookup"><span data-stu-id="fe116-137">You can associate an NSG to subnets and NICs.</span></span> <span data-ttu-id="fe116-138">Můžete také zrušit přidružení skupiny NSG ze všech prostředků, které je přidružené k.</span><span class="sxs-lookup"><span data-stu-id="fe116-138">You can also dissociate an NSG from any resource it's associated to.</span></span>

### <a name="associate-an-nsg-to-a-nic"></a><span data-ttu-id="fe116-139">Přidružení skupiny NSG k síťové karty</span><span class="sxs-lookup"><span data-stu-id="fe116-139">Associate an NSG to a NIC</span></span>
<span data-ttu-id="fe116-140">Pro přidružení **NSG front-endu** NSG k **TestNICWeb1** síťového adaptéru, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fe116-140">To associate the **NSG-FrontEnd** NSG to the **TestNICWeb1** NIC, run the following command:</span></span>

```azurecli
azure network nic set --resource-group RG-NSG \
    --name TestNICWeb1 \
    --network-security-group-name NSG-FrontEnd
```

<span data-ttu-id="fe116-141">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="fe116-141">Expected output:</span></span>

    info:    Executing command network nic set
    + Looking up the network interface "TestNICWeb1"
    + Looking up the network security group "NSG-FrontEnd"
    + Updating network interface "TestNICWeb1"
    + Looking up the network interface "TestNICWeb1"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1
    data:    Name                            : TestNICWeb1
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-30-A1-F8
    data:    Enable IP forwarding            : false
    data:    Tags                            : displayName=NetworkInterfaces - Web
    data:    Network security group          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    data:    Virtual machine                 : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Compute/virtualMachines/Web1
    data:    IP configurations:
    data:      Name                          : ipconfig1
    data:      Provisioning state            : Succeeded
    data:      Public IP address             : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/publicIPAddresses/TestPIPWeb1
    data:      Private IP address            : 192.168.1.5
    data:      Private IP Allocation Method  : Dynamic
    data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

### <a name="dissociate-an-nsg-from-a-nic"></a><span data-ttu-id="fe116-142">Zrušit přidružení skupiny NSG z síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="fe116-142">Dissociate an NSG from a NIC</span></span>

<span data-ttu-id="fe116-143">Zrušení přidružení **NSG front-endu** NSG z **TestNICWeb1** síťového adaptéru, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fe116-143">To dissociate the **NSG-FrontEnd** NSG from the **TestNICWeb1** NIC, run the following command:</span></span>

```azurecli
azure network nic set --resource-group RG-NSG --name TestNICWeb1 --network-security-group-id ""
```

> [!NOTE]
> <span data-ttu-id="fe116-144">Upozornění "" (prázdnou) hodnotu pro `network-security-group-id` parametr.</span><span class="sxs-lookup"><span data-stu-id="fe116-144">Notice the "" (empty) value for the `network-security-group-id` parameter.</span></span> <span data-ttu-id="fe116-145">To je, jak odebrat přidružení skupiny NSG.</span><span class="sxs-lookup"><span data-stu-id="fe116-145">That is how you remove an association to an NSG.</span></span> <span data-ttu-id="fe116-146">Nelze provést totéž s `network-security-group-name` parametr.</span><span class="sxs-lookup"><span data-stu-id="fe116-146">You can't do the same with the `network-security-group-name` parameter.</span></span>
> 

<span data-ttu-id="fe116-147">Očekávaný výsledek:</span><span class="sxs-lookup"><span data-stu-id="fe116-147">Expected result:</span></span>

    info:    Executing command network nic set
    + Looking up the network interface "TestNICWeb1"
    + Updating network interface "TestNICWeb1"
    + Looking up the network interface "TestNICWeb1"
    data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1
    data:    Name                            : TestNICWeb1
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-30-A1-F8
    data:    Enable IP forwarding            : false
    data:    Tags                            : displayName=NetworkInterfaces - Web
    data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Compute/virtualMachines/Web1
    data:    IP configurations:
    data:      Name                          : ipconfig1
    data:      Provisioning state            : Succeeded
    data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/publicIPAddresses/TestPIPWeb1
    data:      Private IP address            : 192.168.1.5
    data:      Private IP Allocation Method  : Dynamic
    data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

### <a name="dissociate-an-nsg-from-a-subnet"></a><span data-ttu-id="fe116-148">Zrušit přidružení skupiny NSG z podsítě</span><span class="sxs-lookup"><span data-stu-id="fe116-148">Dissociate an NSG from a subnet</span></span>
<span data-ttu-id="fe116-149">Zrušení přidružení **NSG front-endu** NSG z **front-endu** podsíť, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fe116-149">To dissociate the **NSG-FrontEnd** NSG from the **FrontEnd** subnet, run the following command:</span></span>

```azurecli
azure network vnet subnet set --resource-group RG-NSG \
    --vnet-name TestVNet \
    --name FrontEnd \
    --network-security-group-id ""
```

<span data-ttu-id="fe116-150">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="fe116-150">Expected output:</span></span>

    info:    Executing command network vnet subnet set
    + Looking up the subnet "FrontEnd"
    + Setting subnet "FrontEnd"
    + Looking up the subnet "FrontEnd"
    data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:    Type                            : Microsoft.Network/virtualNetworks/subnets
    data:    ProvisioningState               : Succeeded
    data:    Name                            : FrontEnd
    data:    Address prefix                  : 192.168.1.0/24
    data:    IP configurations:
    data:      /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1
    data:      /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1
    data:
    info:    network vnet subnet set command OK

### <a name="associate-an-nsg-to-a-subnet"></a><span data-ttu-id="fe116-151">Přidružení skupiny NSG k podsíti</span><span class="sxs-lookup"><span data-stu-id="fe116-151">Associate an NSG to a subnet</span></span>
<span data-ttu-id="fe116-152">Pro přidružení **NSG front-endu** NSG k **FronEnd** podsíť znovu, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fe116-152">To associate the **NSG-FrontEnd** NSG to the **FronEnd** subnet again, run the following command:</span></span>

```azurecli
azure network vnet subnet set --resource-group RG-NSG \
    --vnet-name TestVNet \
    --name FrontEnd \
    --network-security-group-name NSG-FronEnd
```

> [!NOTE]
> <span data-ttu-id="fe116-153">Výše uvedeného příkazu pouze funguje, protože **NSG front-endu** NSG je ve stejné skupině prostředků jako virtuální síť **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="fe116-153">The command above only works because the **NSG-FrontEnd** NSG is in the same resource group as the virtual network **TestVNet**.</span></span> <span data-ttu-id="fe116-154">Pokud NSG v jiné skupině prostředků, budete muset použít `--network-security-group-id` parametr místo toho a poskytnout úplné id skupiny nsg.</span><span class="sxs-lookup"><span data-stu-id="fe116-154">If the NSG is in a different resource group, you need to use the `--network-security-group-id` parameter instead, and provide the full id for the NSG.</span></span> <span data-ttu-id="fe116-155">Id můžete načíst tak, že spustíte `azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd --json` a hledá **id** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="fe116-155">You can retrieve the id by running `azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd --json` and looking for the **id** property.</span></span> 
> 

<span data-ttu-id="fe116-156">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="fe116-156">Expected output:</span></span>

        info:    Executing command network vnet subnet set
        + Looking up the subnet "FrontEnd"
        + Looking up the network security group "NSG-FrontEnd"
        + Setting subnet "FrontEnd"
        + Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    IP configurations:
        data:      /subscriptions/[Subscription Id]resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1
        data:      /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1
        data:
        info:    network vnet subnet set command OK

## <a name="delete-an-nsg"></a><span data-ttu-id="fe116-157">Odstranit skupinu NSG</span><span class="sxs-lookup"><span data-stu-id="fe116-157">Delete an NSG</span></span>
<span data-ttu-id="fe116-158">Skupinu NSG můžete odstranit, pouze pokud má není přidružen k žádnému prostředku.</span><span class="sxs-lookup"><span data-stu-id="fe116-158">You can only delete an NSG if it's not associated to any resource.</span></span> <span data-ttu-id="fe116-159">Pokud chcete odstranit skupinu NSG, postupujte podle následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="fe116-159">To delete an NSG, follow the steps below.</span></span>

1. <span data-ttu-id="fe116-160">Chcete-li zkontrolovat prostředky přidružené k skupinu NSG, spusťte `azure network nsg show` jak je znázorněno v [přidružení skupiny Nsg zobrazení](#View-NSGs-associations).</span><span class="sxs-lookup"><span data-stu-id="fe116-160">To check the resources associated to an NSG, run the `azure network nsg show` as shown in [View NSGs associations](#View-NSGs-associations).</span></span>
2. <span data-ttu-id="fe116-161">Pokud skupina NSG je přidružen k žádné síťové adaptéry, spusťte `azure network nic set` jak je znázorněno v [zrušit přidružení skupiny NSG z síťový adaptér](#Dissociate-an-NSG-from-a-NIC) pro každý síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="fe116-161">If the NSG is associated to any NICs, run the `azure network nic set` as shown in [Dissociate an NSG from a NIC](#Dissociate-an-NSG-from-a-NIC) for each NIC.</span></span> 
3. <span data-ttu-id="fe116-162">Pokud je přidružen k žádné podsíti NSG, spusťte `azure network vnet subnet set` jak je znázorněno v [zrušit přidružení skupiny NSG z podsítě](#Dissociate-an-NSG-from-a-subnet) pro každou podsíť.</span><span class="sxs-lookup"><span data-stu-id="fe116-162">If the NSG is associated to any subnet, run the `azure network vnet subnet set` as shown in [Dissociate an NSG from a subnet](#Dissociate-an-NSG-from-a-subnet) for each subnet.</span></span>
4. <span data-ttu-id="fe116-163">Pokud chcete odstranit NSG, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fe116-163">To delete the NSG, run the following command:</span></span>

    ```azurecli
    azure network nsg delete --resource-group RG-NSG --name NSG-FrontEnd --quiet
    ```

    <span data-ttu-id="fe116-164">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="fe116-164">Expected output:</span></span>

        info:    Executing command network nsg delete
        + Looking up the network security group "NSG-FrontEnd"
        + Deleting network security group "NSG-FrontEnd"
        info:    network nsg delete command OK

## <a name="next-steps"></a><span data-ttu-id="fe116-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fe116-165">Next steps</span></span>
* <span data-ttu-id="fe116-166">[Povolit protokolování](virtual-network-nsg-manage-log.md) pro skupiny Nsg.</span><span class="sxs-lookup"><span data-stu-id="fe116-166">[Enable logging](virtual-network-nsg-manage-log.md) for NSGs.</span></span>

