---
title: "aaaManage skupin zabezpečení - sítě, Azure CLI 2.0 | Microsoft Docs"
description: "Zjistěte, jak hello skupin zabezpečení sítě toomanage pomocí rozhraní příkazového řádku Azure (CLI) 2.0."
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
ms.openlocfilehash: a3036b465e1e4049cba00e5e13ce1b479a2301d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-network-security-groups-using-hello-azure-cli-20"></a><span data-ttu-id="4dc75-103">Správa skupin zabezpečení sítě pomocí hello 2.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="4dc75-103">Manage network security groups using hello Azure CLI 2.0</span></span>

[!INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="4dc75-104">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="4dc75-104">CLI versions toocomplete hello task</span></span> 

<span data-ttu-id="4dc75-105">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="4dc75-105">You can complete hello task using one of hello following CLI versions:</span></span> 

- <span data-ttu-id="4dc75-106">[Azure CLI 1.0](virtual-network-manage-nsg-cli-nodejs.md) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modely</span><span class="sxs-lookup"><span data-stu-id="4dc75-106">[Azure CLI 1.0](virtual-network-manage-nsg-cli-nodejs.md) – our CLI for hello classic and resource management deployment models</span></span> 
- <span data-ttu-id="4dc75-107">[Azure CLI 2.0](#View-existing-NSGs) -naší nové generace rozhraní příkazového řádku pro model nasazení prostředků správu hello (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="4dc75-107">[Azure CLI 2.0](#View-existing-NSGs) - our next generation CLI for hello resource management deployment model (this article)</span></span>


[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="4dc75-108">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="4dc75-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="4dc75-109">Tento článek se zabývá pomocí modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="4dc75-109">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello classic deployment model.</span></span>
> 

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="prerequisite"></a><span data-ttu-id="4dc75-110">Požadavek</span><span class="sxs-lookup"><span data-stu-id="4dc75-110">Prerequisite</span></span>
<span data-ttu-id="4dc75-111">Pokud nebyly dosud, nainstalujete a nakonfigurujete hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="4dc75-111">If you haven't yet, install and configure hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span> 


## <a name="view-existing-nsgs"></a><span data-ttu-id="4dc75-112">Zobrazit existující skupiny Nsg</span><span class="sxs-lookup"><span data-stu-id="4dc75-112">View existing NSGs</span></span>
<span data-ttu-id="4dc75-113">tooview hello seznam skupin Nsg v určité skupiny zdrojů, spusťte hello [seznam nsg sítě az](/cli/azure/network/nsg#list) s `-o table` výstupní formát:</span><span class="sxs-lookup"><span data-stu-id="4dc75-113">tooview hello list of NSGs in a specific resource group, run hello [az network nsg list](/cli/azure/network/nsg#list) command with a `-o table` output format:</span></span>

```azurecli
az network nsg list -g RG-NSG -o table
```

<span data-ttu-id="4dc75-114">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="4dc75-114">Expected output:</span></span>

    Location    Name          ProvisioningState    ResourceGroup    ResourceGuid
    ----------  ------------  -------------------  ---------------  ------------------------------------
    centralus   NSG-BackEnd   Succeeded            RG-NSG           <guid>
    centralus   NSG-FrontEnd  Succeeded            RG-NSG           <guid>

## <a name="list-all-rules-for-an-nsg"></a><span data-ttu-id="4dc75-115">Seznam všech pravidel pro skupiny NSG</span><span class="sxs-lookup"><span data-stu-id="4dc75-115">List all rules for an NSG</span></span>
<span data-ttu-id="4dc75-116">pravidla hello tooview skupinu NSG s názvem **NSG front-endu**spusťte hello [az sítě nsg zobrazit](/cli/azure/network/nsg#show) příkaz pomocí [filtr dotazu JMESPATH](/cli/azure/query-az-cli2) a hello `-o table` výstupní formát:</span><span class="sxs-lookup"><span data-stu-id="4dc75-116">tooview hello rules of an NSG named **NSG-FrontEnd**, run hello [az network nsg show](/cli/azure/network/nsg#show) command using a [JMESPATH query filter](/cli/azure/query-az-cli2) and hello `-o table` output format:</span></span>

```azurecli
    az network nsg show \
    --resource-group RG-NSG \
    --name NSG-FrontEnd \
    --query '[defaultSecurityRules[],securityRules[]][].{Name:name,Desc:description,Access:access,Direction:direction,DestPortRange:destinationPortRange,DestAddrPrefix:destinationAddressPrefix,SrcPortRange:sourcePortRange,SrcAddrPrefix:sourceAddressPrefix}' \
    -o table
```

<span data-ttu-id="4dc75-117">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="4dc75-117">Expected output:</span></span>

    Name                           Desc                                                    Access    Direction    DestPortRange    DestAddrPrefix    SrcPortRange    SrcAddrPrefix
    -----------------------------  ------------------------------------------------------  --------  -----------  ---------------  ----------------  --------------  -----------------
    AllowVnetInBound               Allow inbound traffic from all VMs in VNET              Allow     Inbound      *                VirtualNetwork    *               VirtualNetwork
    AllowAzureLoadBalancerInBound  Allow inbound traffic from azure load balancer          Allow     Inbound      *                *                 *               AzureLoadBalancer
    DenyAllInBound                 Deny all inbound traffic                                Deny      Inbound      *                *                 *               *
    AllowVnetOutBound              Allow outbound traffic from all VMs tooall VMs in VNET  Allow     Outbound     *                VirtualNetwork    *               VirtualNetwork
    AllowInternetOutBound          Allow outbound traffic from all VMs tooInternet         Allow     Outbound     *                Internet          *               *
    DenyAllOutBound                Deny all outbound traffic                               Deny      Outbound     *                *                 *               *
    rdp-rule                                                                               Allow     Inbound      3389             *                 *               Internet
    web-rule                                                                               Allow     Inbound      80               *                 *               Internet
> [!NOTE]
> <span data-ttu-id="4dc75-118">Můžete také použít [seznam pravidel nsg sítě az](/cli/azure/network/nsg/rule#list) toolist pouze hello vlastní pravidla z skupinu NSG.</span><span class="sxs-lookup"><span data-stu-id="4dc75-118">You can also use [az network nsg rule list](/cli/azure/network/nsg/rule#list) toolist only hello custom rules from an NSG .</span></span>
>

## <a name="view-nsg-associations"></a><span data-ttu-id="4dc75-119">Zobrazit přidružení skupiny NSG</span><span class="sxs-lookup"><span data-stu-id="4dc75-119">View NSG associations</span></span>

<span data-ttu-id="4dc75-120">tooview jaké prostředky hello **NSG front-endu** NSG je spojený s, spusťte hello `az network nsg show` příkaz, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="4dc75-120">tooview what resources hello **NSG-FrontEnd** NSG is associate with, run hello `az network nsg show` command as shown below.</span></span> 

```azurecli
az network nsg show -g RG-NSG -n nsg-frontend --query '[subnets,networkInterfaces]'
```

<span data-ttu-id="4dc75-121">Vyhledejte hello **networkInterfaces** a **podsítě** vlastnosti, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="4dc75-121">Look for hello **networkInterfaces** and **subnets** properties as shown below:</span></span>

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

<span data-ttu-id="4dc75-122">V předchozím příkladu hello, hello NSG není přidružené tooany síťových rozhraní (NIC), je přidružené tooa podsíť s názvem **front-endu**.</span><span class="sxs-lookup"><span data-stu-id="4dc75-122">In hello example above, hello NSG is not associated tooany network interfaces (NICs), and it is associated tooa subnet named **FrontEnd**.</span></span>

## <a name="add-a-rule"></a><span data-ttu-id="4dc75-123">Přidání pravidla</span><span class="sxs-lookup"><span data-stu-id="4dc75-123">Add a rule</span></span>
<span data-ttu-id="4dc75-124">pravidlo, které povoluje tooadd **příchozí** tooport provoz **443** z jakékoli toohello počítače **NSG front-endu** NSG, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="4dc75-124">tooadd a rule allowing **inbound** traffic tooport **443** from any machine toohello **NSG-FrontEnd** NSG, enter hello following command:</span></span>

```azurecli
az network nsg rule create  \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd  \
--name allow-https \
--description "Allow access tooport 443 for HTTPS" \
--access Allow \
--protocol Tcp  \
--direction Inbound \
--priority 102 \
--source-address-prefix "*"  \
--source-port-range "*"  \
--destination-address-prefix "*" \
--destination-port-range "443"
```

<span data-ttu-id="4dc75-125">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="4dc75-125">Expected output:</span></span>

```json
{
  "access": "Allow",
  "description": "Allow access tooport 443 for HTTPS",
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

## <a name="change-a-rule"></a><span data-ttu-id="4dc75-126">Změna pravidla</span><span class="sxs-lookup"><span data-stu-id="4dc75-126">Change a rule</span></span>
<span data-ttu-id="4dc75-127">pravidlo hello toochange vytvořili výše tooallow příchozí provoz z hello **Internet** pouze spustit hello [aktualizace pravidla nsg sítě az](/cli/azure/network/nsg/rule#update) příkaz:</span><span class="sxs-lookup"><span data-stu-id="4dc75-127">toochange hello rule created above tooallow inbound traffic from hello **Internet** only, run hello [az network nsg rule update](/cli/azure/network/nsg/rule#update) command:</span></span>

```azurecli
az network nsg rule update \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd \
--name allow-https \
--source-address-prefix Internet
```

<span data-ttu-id="4dc75-128">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="4dc75-128">Expected output:</span></span>

```json
{
"access": "Allow",
"description": "Allow access tooport 443 for HTTPS",
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

## <a name="delete-a-rule"></a><span data-ttu-id="4dc75-129">Odstranění pravidla</span><span class="sxs-lookup"><span data-stu-id="4dc75-129">Delete a rule</span></span>
<span data-ttu-id="4dc75-130">pravidlo hello toodelete vytvořili výše, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="4dc75-130">toodelete hello rule created above, run hello following command:</span></span>

```azurecli
az network nsg rule delete \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd \
--name allow-https
```


## <a name="associate-an-nsg-tooa-nic"></a><span data-ttu-id="4dc75-131">Přidružit NSG tooa síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="4dc75-131">Associate an NSG tooa NIC</span></span>
<span data-ttu-id="4dc75-132">tooassociate hello **NSG front-endu** NSG toohello **TestNICWeb1** síťového adaptéru, použijte hello [aktualizace seskupování sítě az](/cli/azure/network/nic#update) příkaz:</span><span class="sxs-lookup"><span data-stu-id="4dc75-132">tooassociate hello **NSG-FrontEnd** NSG toohello **TestNICWeb1** NIC, use hello [az network nic update](/cli/azure/network/nic#update) command:</span></span>

```azurecli
az network nic update \
--resource-group RG-NSG \
--name TestNICWeb1 \
--network-security-group NSG-FrontEnd    
```

<span data-ttu-id="4dc75-133">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="4dc75-133">Expected output:</span></span>

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

## <a name="dissociate-an-nsg-from-a-nic"></a><span data-ttu-id="4dc75-134">Zrušit přidružení skupiny NSG z síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="4dc75-134">Dissociate an NSG from a NIC</span></span>

<span data-ttu-id="4dc75-135">toodissociate hello **NSG front-endu** NSG z hello **TestNICWeb1** síťového adaptéru, spusťte hello [aktualizace pravidla nsg sítě az](/cli/azure/network/nsg/rule#update) příkaz znovu, ale nahraďte hello `--network-security-group` argument prázdný řetězec (`""`).</span><span class="sxs-lookup"><span data-stu-id="4dc75-135">toodissociate hello **NSG-FrontEnd** NSG from hello **TestNICWeb1** NIC, run hello [az network nsg rule update](/cli/azure/network/nsg/rule#update) command again but replace hello `--network-security-group` argument with an empty string (`""`).</span></span>

```azurecli
az network nic update --resource-group RG-NSG --name TestNICWeb3 --network-security-group ""
```

<span data-ttu-id="4dc75-136">Ve výstupu hello hello `networkSecurityGroup` nastavení toonull klíče.</span><span class="sxs-lookup"><span data-stu-id="4dc75-136">In hello output, hello `networkSecurityGroup` key is set toonull.</span></span>

## <a name="dissociate-an-nsg-from-a-subnet"></a><span data-ttu-id="4dc75-137">Zrušit přidružení skupiny NSG z podsítě</span><span class="sxs-lookup"><span data-stu-id="4dc75-137">Dissociate an NSG from a subnet</span></span>
<span data-ttu-id="4dc75-138">toodissociate hello **NSG front-endu** NSG z hello **front-endu** podsíť, znovu spusťte hello [aktualizace pravidla nsg sítě az](/cli/azure/network/nsg/rule#update) příkaz znovu, ale nahraďte hello `--network-security-group` argument prázdný řetězec (`""`).</span><span class="sxs-lookup"><span data-stu-id="4dc75-138">toodissociate hello **NSG-FrontEnd** NSG from hello **FrontEnd** subnet, again run hello [az network nsg rule update](/cli/azure/network/nsg/rule#update) command again but replace hello `--network-security-group` argument with an empty string (`""`).</span></span>

```azurecli
az network vnet subnet update \
--resource-group RG-NSG \
--vnet-name testvnet \
--name FrontEnd \
--network-security-group ""
```

<span data-ttu-id="4dc75-139">Ve výstupu hello hello `networkSecurityGroup` nastavení toonull klíče.</span><span class="sxs-lookup"><span data-stu-id="4dc75-139">In hello output, hello `networkSecurityGroup` key is set toonull.</span></span>

## <a name="associate-an-nsg-tooa-subnet"></a><span data-ttu-id="4dc75-140">Přidružení podsíť tooa NSG</span><span class="sxs-lookup"><span data-stu-id="4dc75-140">Associate an NSG tooa subnet</span></span>
<span data-ttu-id="4dc75-141">tooassociate hello **NSG front-endu** NSG toohello **front-endu** podsíť znovu spustit hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4dc75-141">tooassociate hello **NSG-FrontEnd** NSG toohello **FrontEnd** subnet again, run hello following command:</span></span>

```azurecli
az network vnet subnet update \
--resource-group RG-NSG \
--vnet-name testvnet \
--name FrontEnd \
--network-security-group NSG-FrontEnd
```

<span data-ttu-id="4dc75-142">Ve výstupu hello hello `networkSecurityGroup` klíč má podobný pro hodnotu hello:</span><span class="sxs-lookup"><span data-stu-id="4dc75-142">In hello output, hello `networkSecurityGroup` key has something similar for hello value:</span></span>

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

## <a name="delete-an-nsg"></a><span data-ttu-id="4dc75-143">Odstranit skupinu NSG</span><span class="sxs-lookup"><span data-stu-id="4dc75-143">Delete an NSG</span></span>
<span data-ttu-id="4dc75-144">Skupinu NSG můžete odstranit, pouze pokud je tooany prostředku není přiřazen.</span><span class="sxs-lookup"><span data-stu-id="4dc75-144">You can only delete an NSG if it's not associated tooany resource.</span></span> <span data-ttu-id="4dc75-145">toodelete skupina NSG, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="4dc75-145">toodelete an NSG, follow hello steps below.</span></span>

1. <span data-ttu-id="4dc75-146">toocheck hello prostředky přidružené tooan NSG, spusťte hello `azure network nsg show` jak je znázorněno v [přidružení skupiny Nsg zobrazení](#View-NSGs-associations).</span><span class="sxs-lookup"><span data-stu-id="4dc75-146">toocheck hello resources associated tooan NSG, run hello `azure network nsg show` as shown in [View NSGs associations](#View-NSGs-associations).</span></span>
2. <span data-ttu-id="4dc75-147">Pokud hello NSG přidružená tooany síťové adaptéry, spusťte hello `azure network nic set` jak je znázorněno v [zrušit přidružení skupiny NSG z síťový adaptér](#Dissociate-an-NSG-from-a-NIC) pro každý síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="4dc75-147">If hello NSG is associated tooany NICs, run hello `azure network nic set` as shown in [Dissociate an NSG from a NIC](#Dissociate-an-NSG-from-a-NIC) for each NIC.</span></span> 
3. <span data-ttu-id="4dc75-148">Pokud hello NSG přidružená tooany podsíť, spusťte hello `azure network vnet subnet set` jak je znázorněno v [zrušit přidružení skupiny NSG z podsítě](#Dissociate-an-NSG-from-a-subnet) pro každou podsíť.</span><span class="sxs-lookup"><span data-stu-id="4dc75-148">If hello NSG is associated tooany subnet, run hello `azure network vnet subnet set` as shown in [Dissociate an NSG from a subnet](#Dissociate-an-NSG-from-a-subnet) for each subnet.</span></span>
4. <span data-ttu-id="4dc75-149">hello toodelete NSG, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="4dc75-149">toodelete hello NSG, run hello following command:</span></span>

    ```azurecli
    az network nsg delete --resource-group RG-NSG --name NSG-FrontEnd
    ```
## <a name="next-steps"></a><span data-ttu-id="4dc75-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4dc75-150">Next steps</span></span>
* <span data-ttu-id="4dc75-151">[Povolit protokolování](virtual-network-nsg-manage-log.md) pro skupiny Nsg.</span><span class="sxs-lookup"><span data-stu-id="4dc75-151">[Enable logging](virtual-network-nsg-manage-log.md) for NSGs.</span></span>

