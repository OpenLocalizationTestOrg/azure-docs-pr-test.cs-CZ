---
title: "Správa skupin zabezpečení sítě - 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Naučte se spravovat skupiny zabezpečení sítě pomocí rozhraní příkazového řádku Azure (CLI) 2.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: ed17d314-07e6-4c7f-bcf1-a8a2535d7c14
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 11ec0d3d9e33c06d4c0a164f7fba5dd5cca73872
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-network-security-groups-using-the-azure-cli-20"></a><span data-ttu-id="39ca2-103">Správa skupin zabezpečení sítě pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="39ca2-103">Manage network security groups using the Azure CLI 2.0</span></span>

[!INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="39ca2-104">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="39ca2-104">CLI versions to complete the task</span></span> 

<span data-ttu-id="39ca2-105">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="39ca2-105">You can complete the task using one of the following CLI versions:</span></span> 

- <span data-ttu-id="39ca2-106">[Azure CLI 1.0](virtual-network-manage-nsg-cli-nodejs.md) – naše rozhraní příkazového řádku pro klasické modely nasazení a modely nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="39ca2-106">[Azure CLI 1.0](virtual-network-manage-nsg-cli-nodejs.md) – our CLI for the classic and resource management deployment models</span></span> 
- <span data-ttu-id="39ca2-107">[Azure CLI 2.0](#View-existing-NSGs) -naší nové generace rozhraní příkazového řádku pro správu model nasazení prostředku (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="39ca2-107">[Azure CLI 2.0](#View-existing-NSGs) - our next generation CLI for the resource management deployment model (this article)</span></span>


[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="39ca2-108">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="39ca2-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="39ca2-109">Tento článek se zabývá pomocí modelu nasazení Resource Manager, které společnost Microsoft doporučuje pro většinu nových nasazení místo modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="39ca2-109">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the classic deployment model.</span></span>
> 

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="prerequisite"></a><span data-ttu-id="39ca2-110">Požadavek</span><span class="sxs-lookup"><span data-stu-id="39ca2-110">Prerequisite</span></span>
<span data-ttu-id="39ca2-111">Pokud nebyly dosud, nainstalovat a nakonfigurovat nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se k Azure účet pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="39ca2-111">If you haven't yet, install and configure the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in to an Azure account using [az login](/cli/azure/#login).</span></span> 


## <a name="view-existing-nsgs"></a><span data-ttu-id="39ca2-112">Zobrazit existující skupiny Nsg</span><span class="sxs-lookup"><span data-stu-id="39ca2-112">View existing NSGs</span></span>
<span data-ttu-id="39ca2-113">Chcete-li zobrazit seznam skupin Nsg v určité skupiny zdrojů, spusťte [seznam nsg sítě az](/cli/azure/network/nsg#list) s `-o table` výstupní formát:</span><span class="sxs-lookup"><span data-stu-id="39ca2-113">To view the list of NSGs in a specific resource group, run the [az network nsg list](/cli/azure/network/nsg#list) command with a `-o table` output format:</span></span>

```azurecli
az network nsg list -g RG-NSG -o table
```

<span data-ttu-id="39ca2-114">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="39ca2-114">Expected output:</span></span>

    Location    Name          ProvisioningState    ResourceGroup    ResourceGuid
    ----------  ------------  -------------------  ---------------  ------------------------------------
    centralus   NSG-BackEnd   Succeeded            RG-NSG           <guid>
    centralus   NSG-FrontEnd  Succeeded            RG-NSG           <guid>

## <a name="list-all-rules-for-an-nsg"></a><span data-ttu-id="39ca2-115">Seznam všech pravidel pro skupiny NSG</span><span class="sxs-lookup"><span data-stu-id="39ca2-115">List all rules for an NSG</span></span>
<span data-ttu-id="39ca2-116">Chcete-li zobrazit pravidla s názvem skupiny NSG **NSG front-endu**spusťte [az sítě nsg zobrazit](/cli/azure/network/nsg#show) příkaz pomocí [JMESPATH filtr dotazu](/cli/azure/query-az-cli2) a `-o table` výstupní formát:</span><span class="sxs-lookup"><span data-stu-id="39ca2-116">To view the rules of an NSG named **NSG-FrontEnd**, run the [az network nsg show](/cli/azure/network/nsg#show) command using a [JMESPATH query filter](/cli/azure/query-az-cli2) and the `-o table` output format:</span></span>

```azurecli
    az network nsg show \
    --resource-group RG-NSG \
    --name NSG-FrontEnd \
    --query '[defaultSecurityRules[],securityRules[]][].{Name:name,Desc:description,Access:access,Direction:direction,DestPortRange:destinationPortRange,DestAddrPrefix:destinationAddressPrefix,SrcPortRange:sourcePortRange,SrcAddrPrefix:sourceAddressPrefix}' \
    -o table
```

<span data-ttu-id="39ca2-117">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="39ca2-117">Expected output:</span></span>

    Name                           Desc                                                    Access    Direction    DestPortRange    DestAddrPrefix    SrcPortRange    SrcAddrPrefix
    -----------------------------  ------------------------------------------------------  --------  -----------  ---------------  ----------------  --------------  -----------------
    AllowVnetInBound               Allow inbound traffic from all VMs in VNET              Allow     Inbound      *                VirtualNetwork    *               VirtualNetwork
    AllowAzureLoadBalancerInBound  Allow inbound traffic from azure load balancer          Allow     Inbound      *                *                 *               AzureLoadBalancer
    DenyAllInBound                 Deny all inbound traffic                                Deny      Inbound      *                *                 *               *
    AllowVnetOutBound              Allow outbound traffic from all VMs to all VMs in VNET  Allow     Outbound     *                VirtualNetwork    *               VirtualNetwork
    AllowInternetOutBound          Allow outbound traffic from all VMs to Internet         Allow     Outbound     *                Internet          *               *
    DenyAllOutBound                Deny all outbound traffic                               Deny      Outbound     *                *                 *               *
    rdp-rule                                                                               Allow     Inbound      3389             *                 *               Internet
    web-rule                                                                               Allow     Inbound      80               *                 *               Internet
> [!NOTE]
> <span data-ttu-id="39ca2-118">Můžete také použít [seznam pravidel nsg sítě az](/cli/azure/network/nsg/rule#list) k zobrazení seznamu pouze vlastní pravidla ze skupiny NSG.</span><span class="sxs-lookup"><span data-stu-id="39ca2-118">You can also use [az network nsg rule list](/cli/azure/network/nsg/rule#list) to list only the custom rules from an NSG .</span></span>
>

## <a name="view-nsg-associations"></a><span data-ttu-id="39ca2-119">Zobrazit přidružení skupiny NSG</span><span class="sxs-lookup"><span data-stu-id="39ca2-119">View NSG associations</span></span>

<span data-ttu-id="39ca2-120">Chcete-li zobrazit prostředky **NSG front-endu** NSG je spojený s spustit `az network nsg show` příkaz, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="39ca2-120">To view what resources the **NSG-FrontEnd** NSG is associate with, run the `az network nsg show` command as shown below.</span></span> 

```azurecli
az network nsg show -g RG-NSG -n nsg-frontend --query '[subnets,networkInterfaces]'
```

<span data-ttu-id="39ca2-121">Vyhledejte **networkInterfaces** a **podsítě** vlastnosti, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="39ca2-121">Look for the **networkInterfaces** and **subnets** properties as shown below:</span></span>

```json
[
  [
    {
      "addressPrefix": null,
      "etag": null,
      "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNET/subnets/FrontEnd",
      "ipConfigurations": null,
      "name": null,
      "networkSecurityGroup": null,
      "provisioningState": null,
      "resourceGroup": "RG-NSG",
      "resourceNavigationLinks": null,
      "routeTable": null
    }
  ],
  null
]
```

<span data-ttu-id="39ca2-122">V předchozím příkladu NSG není přidružen k žádné síťových rozhraní (NIC) a je přidružen k podsíti s názvem **front-endu**.</span><span class="sxs-lookup"><span data-stu-id="39ca2-122">In the example above, the NSG is not associated to any network interfaces (NICs), and it is associated to a subnet named **FrontEnd**.</span></span>

## <a name="add-a-rule"></a><span data-ttu-id="39ca2-123">Přidání pravidla</span><span class="sxs-lookup"><span data-stu-id="39ca2-123">Add a rule</span></span>
<span data-ttu-id="39ca2-124">Chcete-li přidat pravidlo, které povoluje **příchozí** přenosy na portu **443** z libovolného počítače k **NSG front-endu** NSG, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="39ca2-124">To add a rule allowing **inbound** traffic to port **443** from any machine to the **NSG-FrontEnd** NSG, enter the following command:</span></span>

```azurecli
az network nsg rule create  \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd  \
--name allow-https \
--description "Allow access to port 443 for HTTPS" \
--access Allow \
--protocol Tcp  \
--direction Inbound \
--priority 102 \
--source-address-prefix "*"  \
--source-port-range "*"  \
--destination-address-prefix "*" \
--destination-port-range "443"
```

<span data-ttu-id="39ca2-125">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="39ca2-125">Expected output:</span></span>

```json
{
  "access": "Allow",
  "description": "Allow access to port 443 for HTTPS",
  "destinationAddressPrefix": "*",
  "destinationPortRange": "443",
  "direction": "Inbound",
  "etag": "W/\"<guid>\"",
  "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https",
  "name": "allow-https",
  "priority": 102,
  "protocol": "Tcp",
  "provisioningState": "Succeeded",
  "resourceGroup": "RG-NSG",
  "sourceAddressPrefix": "*",
  "sourcePortRange": "*"
}
```

## <a name="change-a-rule"></a><span data-ttu-id="39ca2-126">Změna pravidla</span><span class="sxs-lookup"><span data-stu-id="39ca2-126">Change a rule</span></span>
<span data-ttu-id="39ca2-127">Chcete-li změnit pravidlo vytvořili výše, které pokud chcete povolit příchozí přenosy z **Internet** pouze, spusťte [aktualizace pravidla nsg sítě az](/cli/azure/network/nsg/rule#update) příkaz:</span><span class="sxs-lookup"><span data-stu-id="39ca2-127">To change the rule created above to allow inbound traffic from the **Internet** only, run the [az network nsg rule update](/cli/azure/network/nsg/rule#update) command:</span></span>

```azurecli
az network nsg rule update \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd \
--name allow-https \
--source-address-prefix Internet
```

<span data-ttu-id="39ca2-128">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="39ca2-128">Expected output:</span></span>

```json
{
"access": "Allow",
"description": "Allow access to port 443 for HTTPS",
"destinationAddressPrefix": "*",
"destinationPortRange": "443",
"direction": "Inbound",
"etag": "W/\"<guid>\"",
"id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https",
"name": "allow-https",
"priority": 102,
"protocol": "Tcp",
"provisioningState": "Succeeded",
"resourceGroup": "RG-NSG",
"sourceAddressPrefix": "Internet",
"sourcePortRange": "*"
}
```

## <a name="delete-a-rule"></a><span data-ttu-id="39ca2-129">Odstranění pravidla</span><span class="sxs-lookup"><span data-stu-id="39ca2-129">Delete a rule</span></span>
<span data-ttu-id="39ca2-130">Pokud chcete odstranit pravidlo vytvořili výše, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="39ca2-130">To delete the rule created above, run the following command:</span></span>

```azurecli
az network nsg rule delete \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd \
--name allow-https
```


## <a name="associate-an-nsg-to-a-nic"></a><span data-ttu-id="39ca2-131">Přidružení skupiny NSG k síťové karty</span><span class="sxs-lookup"><span data-stu-id="39ca2-131">Associate an NSG to a NIC</span></span>
<span data-ttu-id="39ca2-132">Pro přidružení **NSG front-endu** NSG k **TestNICWeb1** síťového adaptéru, použijte [aktualizace seskupování sítě az](/cli/azure/network/nic#update) příkaz:</span><span class="sxs-lookup"><span data-stu-id="39ca2-132">To associate the **NSG-FrontEnd** NSG to the **TestNICWeb1** NIC, use the [az network nic update](/cli/azure/network/nic#update) command:</span></span>

```azurecli
az network nic update \
--resource-group RG-NSG \
--name TestNICWeb1 \
--network-security-group NSG-FrontEnd    
```

<span data-ttu-id="39ca2-133">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="39ca2-133">Expected output:</span></span>

```json
{
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": [],
    "internalDnsNameLabel": null,
    "internalDomainNameSuffix": "k0wkaguidnqrh0ud.gx.internal.cloudapp.net",
    "internalFqdn": null
  },
  "enableAcceleratedNetworking": false,
  "enableIpForwarding": false,
  "etag": "W/\"<guid>\"",
  "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1",
  "ipConfigurations": [
    {
      "applicationGatewayBackendAddressPools": null,
      "etag": "W/\"<guid>\"",
      "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1",
      "loadBalancerBackendAddressPools": null,
      "loadBalancerInboundNatRules": null,
      "name": "ipconfig1",
      "primary": true,
      "privateIpAddress": "192.168.1.6",
      "privateIpAddressVersion": "IPv4",
      "privateIpAllocationMethod": "Static",
      "provisioningState": "Succeeded",
      "publicIpAddress": null,
      "resourceGroup": "RG-NSG",
      "subnet": {
        "addressPrefix": null,
        "etag": null,
        "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
        "ipConfigurations": null,
        "name": null,
        "networkSecurityGroup": null,
        "provisioningState": null,
        "resourceGroup": "RG-NSG",
        "resourceNavigationLinks": null,
        "routeTable": null
      }
    }
  ],
  "location": "centralus",
  "macAddress": "00-0D-3A-91-A9-60",
  "name": "TestNICWeb1",
  "networkSecurityGroup": {
    "defaultSecurityRules": null,
    "etag": null,
    "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd",
    "location": null,
    "name": null,
    "networkInterfaces": null,
    "provisioningState": null,
    "resourceGroup": "RG-NSG",
    "resourceGuid": null,
    "securityRules": null,
    "subnets": null,
    "tags": null,
    "type": null
  },
  "primary": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "RG-NSG",
  "resourceGuid": "<guid>",
  "tags": {},
  "type": "Microsoft.Network/networkInterfaces",
  "virtualMachine": null
}
```

## <a name="dissociate-an-nsg-from-a-nic"></a><span data-ttu-id="39ca2-134">Zrušit přidružení skupiny NSG z síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="39ca2-134">Dissociate an NSG from a NIC</span></span>

<span data-ttu-id="39ca2-135">Zrušení přidružení **NSG front-endu** NSG z **TestNICWeb1** síťového adaptéru, spusťte [aktualizace pravidla nsg sítě az](/cli/azure/network/nsg/rule#update) příkaz znovu, ale nahraďte `--network-security-group` argument prázdný řetězec (`""`).</span><span class="sxs-lookup"><span data-stu-id="39ca2-135">To dissociate the **NSG-FrontEnd** NSG from the **TestNICWeb1** NIC, run the [az network nsg rule update](/cli/azure/network/nsg/rule#update) command again but replace the `--network-security-group` argument with an empty string (`""`).</span></span>

```azurecli
az network nic update --resource-group RG-NSG --name TestNICWeb3 --network-security-group ""
```

<span data-ttu-id="39ca2-136">Ve výstupu `networkSecurityGroup` klíč je nastaven na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="39ca2-136">In the output, the `networkSecurityGroup` key is set to null.</span></span>

## <a name="dissociate-an-nsg-from-a-subnet"></a><span data-ttu-id="39ca2-137">Zrušit přidružení skupiny NSG z podsítě</span><span class="sxs-lookup"><span data-stu-id="39ca2-137">Dissociate an NSG from a subnet</span></span>
<span data-ttu-id="39ca2-138">Zrušení přidružení **NSG front-endu** NSG z **front-endu** podsíť, znovu spustit [aktualizace pravidla nsg sítě az](/cli/azure/network/nsg/rule#update) příkaz znovu, ale nahraďte `--network-security-group` argument prázdný řetězec (`""`).</span><span class="sxs-lookup"><span data-stu-id="39ca2-138">To dissociate the **NSG-FrontEnd** NSG from the **FrontEnd** subnet, again run the [az network nsg rule update](/cli/azure/network/nsg/rule#update) command again but replace the `--network-security-group` argument with an empty string (`""`).</span></span>

```azurecli
az network vnet subnet update \
--resource-group RG-NSG \
--vnet-name testvnet \
--name FrontEnd \
--network-security-group ""
```

<span data-ttu-id="39ca2-139">Ve výstupu `networkSecurityGroup` klíč je nastaven na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="39ca2-139">In the output, the `networkSecurityGroup` key is set to null.</span></span>

## <a name="associate-an-nsg-to-a-subnet"></a><span data-ttu-id="39ca2-140">Přidružení skupiny NSG k podsíti</span><span class="sxs-lookup"><span data-stu-id="39ca2-140">Associate an NSG to a subnet</span></span>
<span data-ttu-id="39ca2-141">Pro přidružení **NSG front-endu** NSG k **front-endu** podsíť znovu, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="39ca2-141">To associate the **NSG-FrontEnd** NSG to the **FrontEnd** subnet again, run the following command:</span></span>

```azurecli
az network vnet subnet update \
--resource-group RG-NSG \
--vnet-name testvnet \
--name FrontEnd \
--network-security-group NSG-FrontEnd
```

<span data-ttu-id="39ca2-142">Ve výstupu `networkSecurityGroup` klíč má podobný pro hodnotu:</span><span class="sxs-lookup"><span data-stu-id="39ca2-142">In the output, the `networkSecurityGroup` key has something similar for the value:</span></span>

```json
"networkSecurityGroup": {
    "defaultSecurityRules": null,
    "etag": null,
    "id": "/subscriptions/0e220bf6-5caa-4e9f-8383-51f16b6c109f/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd",
    "location": null,
    "name": null,
    "networkInterfaces": null,
    "provisioningState": null,
    "resourceGroup": "RG-NSG",
    "resourceGuid": null,
    "securityRules": null,
    "subnets": null,
    "tags": null,
    "type": null
  }
  ```

## <a name="delete-an-nsg"></a><span data-ttu-id="39ca2-143">Odstranit skupinu NSG</span><span class="sxs-lookup"><span data-stu-id="39ca2-143">Delete an NSG</span></span>
<span data-ttu-id="39ca2-144">Skupinu NSG můžete odstranit, pouze pokud má není přidružen k žádnému prostředku.</span><span class="sxs-lookup"><span data-stu-id="39ca2-144">You can only delete an NSG if it's not associated to any resource.</span></span> <span data-ttu-id="39ca2-145">Pokud chcete odstranit skupinu NSG, postupujte podle následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="39ca2-145">To delete an NSG, follow the steps below.</span></span>

1. <span data-ttu-id="39ca2-146">Chcete-li zkontrolovat prostředky přidružené k skupinu NSG, spusťte `azure network nsg show` jak je znázorněno v [přidružení skupiny Nsg zobrazení](#View-NSGs-associations).</span><span class="sxs-lookup"><span data-stu-id="39ca2-146">To check the resources associated to an NSG, run the `azure network nsg show` as shown in [View NSGs associations](#View-NSGs-associations).</span></span>
2. <span data-ttu-id="39ca2-147">Pokud skupina NSG je přidružen k žádné síťové adaptéry, spusťte `azure network nic set` jak je znázorněno v [zrušit přidružení skupiny NSG z síťový adaptér](#Dissociate-an-NSG-from-a-NIC) pro každý síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="39ca2-147">If the NSG is associated to any NICs, run the `azure network nic set` as shown in [Dissociate an NSG from a NIC](#Dissociate-an-NSG-from-a-NIC) for each NIC.</span></span> 
3. <span data-ttu-id="39ca2-148">Pokud je přidružen k žádné podsíti NSG, spusťte `azure network vnet subnet set` jak je znázorněno v [zrušit přidružení skupiny NSG z podsítě](#Dissociate-an-NSG-from-a-subnet) pro každou podsíť.</span><span class="sxs-lookup"><span data-stu-id="39ca2-148">If the NSG is associated to any subnet, run the `azure network vnet subnet set` as shown in [Dissociate an NSG from a subnet](#Dissociate-an-NSG-from-a-subnet) for each subnet.</span></span>
4. <span data-ttu-id="39ca2-149">Pokud chcete odstranit NSG, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="39ca2-149">To delete the NSG, run the following command:</span></span>

    ```azurecli
    az network nsg delete --resource-group RG-NSG --name NSG-FrontEnd
    ```
## <a name="next-steps"></a><span data-ttu-id="39ca2-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="39ca2-150">Next steps</span></span>
* <span data-ttu-id="39ca2-151">[Povolit protokolování](virtual-network-nsg-manage-log.md) pro skupiny Nsg.</span><span class="sxs-lookup"><span data-stu-id="39ca2-151">[Enable logging](virtual-network-nsg-manage-log.md) for NSGs.</span></span>

