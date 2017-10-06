---
title: "aaaManage skupin zabezpečení - sítě, Azure CLI 1.0 | Microsoft Docs"
description: "Zjistěte, jak hello skupin zabezpečení sítě toomanage pomocí rozhraní příkazového řádku Azure (CLI) 1.0."
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
ms.openlocfilehash: 9a429f947abbcb5fa6adb40c84504f68efd5e20e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-network-security-groups-using-hello-azure-cli-10"></a><span data-ttu-id="d8082-103">Správa skupin zabezpečení sítě pomocí hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="d8082-103">Manage network security groups using hello Azure CLI 1.0</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="d8082-104">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="d8082-104">CLI versions toocomplete hello task</span></span> 

<span data-ttu-id="d8082-105">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="d8082-105">You can complete hello task using one of hello following CLI versions:</span></span> 

- <span data-ttu-id="d8082-106">[Azure CLI 1.0](#View-existing-NSGs) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modely</span><span class="sxs-lookup"><span data-stu-id="d8082-106">[Azure CLI 1.0](#View-existing-NSGs) – our CLI for hello classic and resource management deployment models</span></span> 
- <span data-ttu-id="d8082-107">[Azure CLI 2.0](virtual-network-manage-nsg-arm-cli.md) -naší nové generace rozhraní příkazového řádku pro model nasazení prostředků správu hello (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="d8082-107">[Azure CLI 2.0](virtual-network-manage-nsg-arm-cli.md) - our next generation CLI for hello resource management deployment model (this article)</span></span>

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="d8082-108">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="d8082-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="d8082-109">Tento článek se zabývá pomocí modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="d8082-109">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello classic deployment model.</span></span>
> 

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="retrieve-information"></a><span data-ttu-id="d8082-110">Načtení informací o</span><span class="sxs-lookup"><span data-stu-id="d8082-110">Retrieve Information</span></span>
<span data-ttu-id="d8082-111">Můžete zobrazit stávající skupiny Nsg, načíst pravidla pro existující skupiny NSG a zjistit, jaké prostředky skupinu NSG je přidružena k.</span><span class="sxs-lookup"><span data-stu-id="d8082-111">You can view your existing NSGs, retrieve rules for an existing NSG, and find out what resources an NSG is associated to.</span></span>

### <a name="view-existing-nsgs"></a><span data-ttu-id="d8082-112">Zobrazit existující skupiny Nsg</span><span class="sxs-lookup"><span data-stu-id="d8082-112">View existing NSGs</span></span>
<span data-ttu-id="d8082-113">tooview hello seznam skupin Nsg v určité skupiny zdrojů, spusťte hello `azure network nsg list` příkaz, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="d8082-113">tooview hello list of NSGs in a specific resource group, run hello `azure network nsg list` command as shown below.</span></span>

```azurecli
azure network nsg list --resource-group RG-NSG
```

<span data-ttu-id="d8082-114">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="d8082-114">Expected output:</span></span>

    info:    Executing command network nsg list
    + Getting hello network security groups
    data:    Name          Location
    data:    ------------  --------
    data:    NSG-BackEnd   westus
    data:    NSG-FrontEnd  westus
    info:    network nsg list command OK

### <a name="list-all-rules-for-an-nsg"></a><span data-ttu-id="d8082-115">Seznam všech pravidel pro skupiny NSG</span><span class="sxs-lookup"><span data-stu-id="d8082-115">List all rules for an NSG</span></span>
<span data-ttu-id="d8082-116">pravidla hello tooview skupinu NSG s názvem **NSG front-endu**spusťte hello `azure network nsg show` příkaz, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="d8082-116">tooview hello rules of an NSG named **NSG-FrontEnd**, run hello `azure network nsg show` command as shown below.</span></span> 

```azurecli
azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd
```

<span data-ttu-id="d8082-117">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="d8082-117">Expected output:</span></span>

    info:    Executing command network nsg show
    + Looking up hello network security group "NSG-FrontEnd"
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
> <span data-ttu-id="d8082-118">Můžete také použít `azure network nsg rule list --resource-group RG-NSG --nsg-name NSG-FrontEnd` toolist hello pravidla z hello **NSG front-endu** NSG.</span><span class="sxs-lookup"><span data-stu-id="d8082-118">You can also use `azure network nsg rule list --resource-group RG-NSG --nsg-name NSG-FrontEnd` toolist hello rules from hello **NSG-FrontEnd** NSG.</span></span>
>

### <a name="view-nsg-associations"></a><span data-ttu-id="d8082-119">Zobrazit přidružení skupiny NSG</span><span class="sxs-lookup"><span data-stu-id="d8082-119">View NSG associations</span></span>

<span data-ttu-id="d8082-120">tooview jaké prostředky hello **NSG front-endu** NSG je spojený s, spusťte hello `azure network nsg show` příkaz, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="d8082-120">tooview what resources hello **NSG-FrontEnd** NSG is associate with, run hello `azure network nsg show` command as shown below.</span></span> <span data-ttu-id="d8082-121">Všimněte si, že hello jenom rozdíl je použití hello hello **– json** parametr.</span><span class="sxs-lookup"><span data-stu-id="d8082-121">Notice that hello only difference is hello use of hello **--json** parameter.</span></span>

```azurecli
azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd --json
```

<span data-ttu-id="d8082-122">Vyhledejte hello **networkInterfaces** a **podsítě** vlastnosti, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="d8082-122">Look for hello **networkInterfaces** and **subnets** properties as shown below:</span></span>

    "networkInterfaces": [],
    ...
    "subnets": [
        {
            "id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
        }
    ],
    ...

<span data-ttu-id="d8082-123">V předchozím příkladu hello, hello NSG není přidružené tooany síťových rozhraní (NIC), je přidružené tooa podsíť s názvem **front-endu**.</span><span class="sxs-lookup"><span data-stu-id="d8082-123">In hello example above, hello NSG is not associated tooany network interfaces (NICs), and it is associated tooa subnet named **FrontEnd**.</span></span>

## <a name="manage-rules"></a><span data-ttu-id="d8082-124">Spravovat pravidla</span><span class="sxs-lookup"><span data-stu-id="d8082-124">Manage rules</span></span>
<span data-ttu-id="d8082-125">Můžete přidat pravidla tooan existující skupina NSG, upravit stávající pravidla a odstranit pravidla.</span><span class="sxs-lookup"><span data-stu-id="d8082-125">You can add rules tooan existing NSG, edit existing rules, and remove rules.</span></span>

### <a name="add-a-rule"></a><span data-ttu-id="d8082-126">Přidání pravidla</span><span class="sxs-lookup"><span data-stu-id="d8082-126">Add a rule</span></span>
<span data-ttu-id="d8082-127">pravidlo, které povoluje tooadd **příchozí** tooport provoz **443** z jakékoli toohello počítače **NSG front-endu** NSG, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="d8082-127">tooadd a rule allowing **inbound** traffic tooport **443** from any machine toohello **NSG-FrontEnd** NSG, enter hello following command:</span></span>

```azurecli
azure network nsg rule create --resource-group RG-NSG \
    --nsg-name NSG-FrontEnd \
    --name allow-https \
    --description "Allow access tooport 443 for HTTPS" \
    --protocol Tcp \
    --source-address-prefix * \
    --source-port-range * \
    --destination-address-prefix * \
    --destination-port-range 443 \
    --access Allow \
    --priority 102 \
    --direction Inbound
```

<span data-ttu-id="d8082-128">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="d8082-128">Expected output:</span></span>

    info:    Executing command network nsg rule create
    + Looking up hello network security rule "allow-https"
    + Creating a network security rule "allow-https"
    + Looking up hello network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https
    data:    Name                            : allow-https
    data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
    data:    Provisioning state              : Succeeded
    data:    Description                     : Allow access tooport 443 for HTTPS
    data:    Source IP                       : *
    data:    Source Port                     : *
    data:    Destination IP                  : *
    data:    Destination Port                : 443
    data:    Protocol                        : Tcp
    data:    Direction                       : Inbound
    data:    Access                          : Allow
    data:    Priority                        : 102
    info:    network nsg rule create command OK

### <a name="change-a-rule"></a><span data-ttu-id="d8082-129">Změna pravidla</span><span class="sxs-lookup"><span data-stu-id="d8082-129">Change a rule</span></span>
<span data-ttu-id="d8082-130">pravidlo hello toochange vytvořili výše tooallow příchozí provoz z hello **Internet** pouze, spusťte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d8082-130">toochange hello rule created above tooallow inbound traffic from hello **Internet** only, run hello following command:</span></span>

```azurecli
azure network nsg rule set --resource-group RG-NSG \
    --nsg-name NSG-FrontEnd \
    --name allow-https \
    --source-address-prefix Internet
```

<span data-ttu-id="d8082-131">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="d8082-131">Expected output:</span></span>

    info:    Executing command network nsg rule set
    + Looking up hello network security group "NSG-FrontEnd"
    + Setting a network security rule "allow-https"
    + Looking up hello network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https
    data:    Name                            : allow-https
    data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
    data:    Provisioning state              : Succeeded
    data:    Description                     : Allow access tooport 443 for HTTPS
    data:    Source IP                       : Internet
    data:    Source Port                     : *
    data:    Destination IP                  : *
    data:    Destination Port                : 443
    data:    Protocol                        : Tcp
    data:    Direction                       : Inbound
    data:    Access                          : Allow
    data:    Priority                        : 102
    info:    network nsg rule set command OK

### <a name="delete-a-rule"></a><span data-ttu-id="d8082-132">Odstranění pravidla</span><span class="sxs-lookup"><span data-stu-id="d8082-132">Delete a rule</span></span>
<span data-ttu-id="d8082-133">pravidlo hello toodelete vytvořili výše, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="d8082-133">toodelete hello rule created above, run hello following command:</span></span>

```azurecli
azure network nsg rule delete --resource-group RG-NSG \
    --nsg-name NSG-FrontEnd \
    --name allow-https \
    --quiet
```

> [!NOTE]
> <span data-ttu-id="d8082-134">Hello `--quiet` parametr zajišťuje nepotřebujete tooconfirm hello odstranění.</span><span class="sxs-lookup"><span data-stu-id="d8082-134">hello `--quiet` parameter ensures you don't need tooconfirm hello deletion.</span></span>
>

<span data-ttu-id="d8082-135">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="d8082-135">Expected output:</span></span>

    info:    Executing command network nsg rule delete
    + Looking up hello network security group "NSG-FrontEnd"
    + Deleting network security rule "allow-https"
    info:    network nsg rule delete command OK

## <a name="manage-associations"></a><span data-ttu-id="d8082-136">Správa přidružení</span><span class="sxs-lookup"><span data-stu-id="d8082-136">Manage associations</span></span>
<span data-ttu-id="d8082-137">Můžete přidružit toosubnets NSG a síťových karet.</span><span class="sxs-lookup"><span data-stu-id="d8082-137">You can associate an NSG toosubnets and NICs.</span></span> <span data-ttu-id="d8082-138">Můžete také zrušit přidružení skupiny NSG ze všech prostředků, které je přidružené k.</span><span class="sxs-lookup"><span data-stu-id="d8082-138">You can also dissociate an NSG from any resource it's associated to.</span></span>

### <a name="associate-an-nsg-tooa-nic"></a><span data-ttu-id="d8082-139">Přidružit NSG tooa síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="d8082-139">Associate an NSG tooa NIC</span></span>
<span data-ttu-id="d8082-140">tooassociate hello **NSG front-endu** NSG toohello **TestNICWeb1** síťového adaptéru, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="d8082-140">tooassociate hello **NSG-FrontEnd** NSG toohello **TestNICWeb1** NIC, run hello following command:</span></span>

```azurecli
azure network nic set --resource-group RG-NSG \
    --name TestNICWeb1 \
    --network-security-group-name NSG-FrontEnd
```

<span data-ttu-id="d8082-141">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="d8082-141">Expected output:</span></span>

    info:    Executing command network nic set
    + Looking up hello network interface "TestNICWeb1"
    + Looking up hello network security group "NSG-FrontEnd"
    + Updating network interface "TestNICWeb1"
    + Looking up hello network interface "TestNICWeb1"
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

### <a name="dissociate-an-nsg-from-a-nic"></a><span data-ttu-id="d8082-142">Zrušit přidružení skupiny NSG z síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="d8082-142">Dissociate an NSG from a NIC</span></span>

<span data-ttu-id="d8082-143">toodissociate hello **NSG front-endu** NSG z hello **TestNICWeb1** síťového adaptéru, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="d8082-143">toodissociate hello **NSG-FrontEnd** NSG from hello **TestNICWeb1** NIC, run hello following command:</span></span>

```azurecli
azure network nic set --resource-group RG-NSG --name TestNICWeb1 --network-security-group-id ""
```

> [!NOTE]
> <span data-ttu-id="d8082-144">Všimněte si hello "" (prázdnou) hodnotu pro hello `network-security-group-id` parametr.</span><span class="sxs-lookup"><span data-stu-id="d8082-144">Notice hello "" (empty) value for hello `network-security-group-id` parameter.</span></span> <span data-ttu-id="d8082-145">To je, jak odebrat tooan přidružení skupiny NSG.</span><span class="sxs-lookup"><span data-stu-id="d8082-145">That is how you remove an association tooan NSG.</span></span> <span data-ttu-id="d8082-146">Nelze provést stejný hello s hello `network-security-group-name` parametr.</span><span class="sxs-lookup"><span data-stu-id="d8082-146">You can't do hello same with hello `network-security-group-name` parameter.</span></span>
> 

<span data-ttu-id="d8082-147">Očekávaný výsledek:</span><span class="sxs-lookup"><span data-stu-id="d8082-147">Expected result:</span></span>

    info:    Executing command network nic set
    + Looking up hello network interface "TestNICWeb1"
    + Updating network interface "TestNICWeb1"
    + Looking up hello network interface "TestNICWeb1"
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

### <a name="dissociate-an-nsg-from-a-subnet"></a><span data-ttu-id="d8082-148">Zrušit přidružení skupiny NSG z podsítě</span><span class="sxs-lookup"><span data-stu-id="d8082-148">Dissociate an NSG from a subnet</span></span>
<span data-ttu-id="d8082-149">toodissociate hello **NSG front-endu** NSG z hello **front-endu** podsíť, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="d8082-149">toodissociate hello **NSG-FrontEnd** NSG from hello **FrontEnd** subnet, run hello following command:</span></span>

```azurecli
azure network vnet subnet set --resource-group RG-NSG \
    --vnet-name TestVNet \
    --name FrontEnd \
    --network-security-group-id ""
```

<span data-ttu-id="d8082-150">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="d8082-150">Expected output:</span></span>

    info:    Executing command network vnet subnet set
    + Looking up hello subnet "FrontEnd"
    + Setting subnet "FrontEnd"
    + Looking up hello subnet "FrontEnd"
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

### <a name="associate-an-nsg-tooa-subnet"></a><span data-ttu-id="d8082-151">Přidružení podsíť tooa NSG</span><span class="sxs-lookup"><span data-stu-id="d8082-151">Associate an NSG tooa subnet</span></span>
<span data-ttu-id="d8082-152">tooassociate hello **NSG front-endu** NSG toohello **FronEnd** podsíť znovu spustit hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d8082-152">tooassociate hello **NSG-FrontEnd** NSG toohello **FronEnd** subnet again, run hello following command:</span></span>

```azurecli
azure network vnet subnet set --resource-group RG-NSG \
    --vnet-name TestVNet \
    --name FrontEnd \
    --network-security-group-name NSG-FronEnd
```

> [!NOTE]
> <span data-ttu-id="d8082-153">Hello nahoře na příkaz pouze funguje, protože hello **NSG front-endu** NSG se hello stejné skupině prostředků jako virtuální síť hello **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="d8082-153">hello command above only works because hello **NSG-FrontEnd** NSG is in hello same resource group as hello virtual network **TestVNet**.</span></span> <span data-ttu-id="d8082-154">Pokud hello NSG se nachází v jiné skupině prostředků, je třeba toouse hello `--network-security-group-id` parametr místo toho a poskytnout úplné id hello hello NSG.</span><span class="sxs-lookup"><span data-stu-id="d8082-154">If hello NSG is in a different resource group, you need toouse hello `--network-security-group-id` parameter instead, and provide hello full id for hello NSG.</span></span> <span data-ttu-id="d8082-155">Hello id můžete načíst tak, že spustíte `azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd --json` a hledá hello **id** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="d8082-155">You can retrieve hello id by running `azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd --json` and looking for hello **id** property.</span></span> 
> 

<span data-ttu-id="d8082-156">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="d8082-156">Expected output:</span></span>

        info:    Executing command network vnet subnet set
        + Looking up hello subnet "FrontEnd"
        + Looking up hello network security group "NSG-FrontEnd"
        + Setting subnet "FrontEnd"
        + Looking up hello subnet "FrontEnd"
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

## <a name="delete-an-nsg"></a><span data-ttu-id="d8082-157">Odstranit skupinu NSG</span><span class="sxs-lookup"><span data-stu-id="d8082-157">Delete an NSG</span></span>
<span data-ttu-id="d8082-158">Skupinu NSG můžete odstranit, pouze pokud je tooany prostředku není přiřazen.</span><span class="sxs-lookup"><span data-stu-id="d8082-158">You can only delete an NSG if it's not associated tooany resource.</span></span> <span data-ttu-id="d8082-159">toodelete skupina NSG, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="d8082-159">toodelete an NSG, follow hello steps below.</span></span>

1. <span data-ttu-id="d8082-160">toocheck hello prostředky přidružené tooan NSG, spusťte hello `azure network nsg show` jak je znázorněno v [přidružení skupiny Nsg zobrazení](#View-NSGs-associations).</span><span class="sxs-lookup"><span data-stu-id="d8082-160">toocheck hello resources associated tooan NSG, run hello `azure network nsg show` as shown in [View NSGs associations](#View-NSGs-associations).</span></span>
2. <span data-ttu-id="d8082-161">Pokud hello NSG přidružená tooany síťové adaptéry, spusťte hello `azure network nic set` jak je znázorněno v [zrušit přidružení skupiny NSG z síťový adaptér](#Dissociate-an-NSG-from-a-NIC) pro každý síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="d8082-161">If hello NSG is associated tooany NICs, run hello `azure network nic set` as shown in [Dissociate an NSG from a NIC](#Dissociate-an-NSG-from-a-NIC) for each NIC.</span></span> 
3. <span data-ttu-id="d8082-162">Pokud hello NSG přidružená tooany podsíť, spusťte hello `azure network vnet subnet set` jak je znázorněno v [zrušit přidružení skupiny NSG z podsítě](#Dissociate-an-NSG-from-a-subnet) pro každou podsíť.</span><span class="sxs-lookup"><span data-stu-id="d8082-162">If hello NSG is associated tooany subnet, run hello `azure network vnet subnet set` as shown in [Dissociate an NSG from a subnet](#Dissociate-an-NSG-from-a-subnet) for each subnet.</span></span>
4. <span data-ttu-id="d8082-163">hello toodelete NSG, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="d8082-163">toodelete hello NSG, run hello following command:</span></span>

    ```azurecli
    azure network nsg delete --resource-group RG-NSG --name NSG-FrontEnd --quiet
    ```

    <span data-ttu-id="d8082-164">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="d8082-164">Expected output:</span></span>

        info:    Executing command network nsg delete
        + Looking up hello network security group "NSG-FrontEnd"
        + Deleting network security group "NSG-FrontEnd"
        info:    network nsg delete command OK

## <a name="next-steps"></a><span data-ttu-id="d8082-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d8082-165">Next steps</span></span>
* <span data-ttu-id="d8082-166">[Povolit protokolování](virtual-network-nsg-manage-log.md) pro skupiny Nsg.</span><span class="sxs-lookup"><span data-stu-id="d8082-166">[Enable logging](virtual-network-nsg-manage-log.md) for NSGs.</span></span>

