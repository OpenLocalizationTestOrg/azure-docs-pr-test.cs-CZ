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
# <a name="manage-network-security-groups-using-hello-azure-cli-20"></a>Správa skupin zabezpečení sítě pomocí hello 2.0 rozhraní příkazového řádku Azure

[!INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku 

Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku: 

- [Azure CLI 1.0](virtual-network-manage-nsg-cli-nodejs.md) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modely 
- [Azure CLI 2.0](#View-existing-NSGs) -naší nové generace rozhraní příkazového řádku pro model nasazení prostředků správu hello (v tomto článku)


[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello modelu nasazení classic.
> 

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="prerequisite"></a>Požadavek
Pokud nebyly dosud, nainstalujete a nakonfigurujete hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login). 


## <a name="view-existing-nsgs"></a>Zobrazit existující skupiny Nsg
tooview hello seznam skupin Nsg v určité skupiny zdrojů, spusťte hello [seznam nsg sítě az](/cli/azure/network/nsg#list) s `-o table` výstupní formát:

```azurecli
az network nsg list -g RG-NSG -o table
```

Očekávaný výstup:

    Location    Name          ProvisioningState    ResourceGroup    ResourceGuid
    ----------  ------------  -------------------  ---------------  ------------------------------------
    centralus   NSG-BackEnd   Succeeded            RG-NSG           <guid>
    centralus   NSG-FrontEnd  Succeeded            RG-NSG           <guid>

## <a name="list-all-rules-for-an-nsg"></a>Seznam všech pravidel pro skupiny NSG
pravidla hello tooview skupinu NSG s názvem **NSG front-endu**spusťte hello [az sítě nsg zobrazit](/cli/azure/network/nsg#show) příkaz pomocí [filtr dotazu JMESPATH](/cli/azure/query-az-cli2) a hello `-o table` výstupní formát:

```azurecli
    az network nsg show \
    --resource-group RG-NSG \
    --name NSG-FrontEnd \
    --query '[defaultSecurityRules[],securityRules[]][].{Name:name,Desc:description,Access:access,Direction:direction,DestPortRange:destinationPortRange,DestAddrPrefix:destinationAddressPrefix,SrcPortRange:sourcePortRange,SrcAddrPrefix:sourceAddressPrefix}' \
    -o table
```

Očekávaný výstup:

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
> Můžete také použít [seznam pravidel nsg sítě az](/cli/azure/network/nsg/rule#list) toolist pouze hello vlastní pravidla z skupinu NSG.
>

## <a name="view-nsg-associations"></a>Zobrazit přidružení skupiny NSG

tooview jaké prostředky hello **NSG front-endu** NSG je spojený s, spusťte hello `az network nsg show` příkaz, jak je uvedeno níže. 

```azurecli
az network nsg show -g RG-NSG -n nsg-frontend --query '[subnets,networkInterfaces]'
```

Vyhledejte hello **networkInterfaces** a **podsítě** vlastnosti, jak je uvedeno níže:

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

V předchozím příkladu hello, hello NSG není přidružené tooany síťových rozhraní (NIC), je přidružené tooa podsíť s názvem **front-endu**.

## <a name="add-a-rule"></a>Přidání pravidla
pravidlo, které povoluje tooadd **příchozí** tooport provoz **443** z jakékoli toohello počítače **NSG front-endu** NSG, zadejte následující příkaz hello:

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

Očekávaný výstup:

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

## <a name="change-a-rule"></a>Změna pravidla
pravidlo hello toochange vytvořili výše tooallow příchozí provoz z hello **Internet** pouze spustit hello [aktualizace pravidla nsg sítě az](/cli/azure/network/nsg/rule#update) příkaz:

```azurecli
az network nsg rule update \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd \
--name allow-https \
--source-address-prefix Internet
```

Očekávaný výstup:

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

## <a name="delete-a-rule"></a>Odstranění pravidla
pravidlo hello toodelete vytvořili výše, spusťte následující příkaz hello:

```azurecli
az network nsg rule delete \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd \
--name allow-https
```


## <a name="associate-an-nsg-tooa-nic"></a>Přidružit NSG tooa síťový adaptér
tooassociate hello **NSG front-endu** NSG toohello **TestNICWeb1** síťového adaptéru, použijte hello [aktualizace seskupování sítě az](/cli/azure/network/nic#update) příkaz:

```azurecli
az network nic update \
--resource-group RG-NSG \
--name TestNICWeb1 \
--network-security-group NSG-FrontEnd    
```

Očekávaný výstup:

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

## <a name="dissociate-an-nsg-from-a-nic"></a>Zrušit přidružení skupiny NSG z síťový adaptér

toodissociate hello **NSG front-endu** NSG z hello **TestNICWeb1** síťového adaptéru, spusťte hello [aktualizace pravidla nsg sítě az](/cli/azure/network/nsg/rule#update) příkaz znovu, ale nahraďte hello `--network-security-group` argument prázdný řetězec (`""`).

```azurecli
az network nic update --resource-group RG-NSG --name TestNICWeb3 --network-security-group ""
```

Ve výstupu hello hello `networkSecurityGroup` nastavení toonull klíče.

## <a name="dissociate-an-nsg-from-a-subnet"></a>Zrušit přidružení skupiny NSG z podsítě
toodissociate hello **NSG front-endu** NSG z hello **front-endu** podsíť, znovu spusťte hello [aktualizace pravidla nsg sítě az](/cli/azure/network/nsg/rule#update) příkaz znovu, ale nahraďte hello `--network-security-group` argument prázdný řetězec (`""`).

```azurecli
az network vnet subnet update \
--resource-group RG-NSG \
--vnet-name testvnet \
--name FrontEnd \
--network-security-group ""
```

Ve výstupu hello hello `networkSecurityGroup` nastavení toonull klíče.

## <a name="associate-an-nsg-tooa-subnet"></a>Přidružení podsíť tooa NSG
tooassociate hello **NSG front-endu** NSG toohello **front-endu** podsíť znovu spustit hello následující příkaz:

```azurecli
az network vnet subnet update \
--resource-group RG-NSG \
--vnet-name testvnet \
--name FrontEnd \
--network-security-group NSG-FrontEnd
```

Ve výstupu hello hello `networkSecurityGroup` klíč má podobný pro hodnotu hello:

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

## <a name="delete-an-nsg"></a>Odstranit skupinu NSG
Skupinu NSG můžete odstranit, pouze pokud je tooany prostředku není přiřazen. toodelete skupina NSG, postupujte podle následujících kroků hello.

1. toocheck hello prostředky přidružené tooan NSG, spusťte hello `azure network nsg show` jak je znázorněno v [přidružení skupiny Nsg zobrazení](#View-NSGs-associations).
2. Pokud hello NSG přidružená tooany síťové adaptéry, spusťte hello `azure network nic set` jak je znázorněno v [zrušit přidružení skupiny NSG z síťový adaptér](#Dissociate-an-NSG-from-a-NIC) pro každý síťový adaptér. 
3. Pokud hello NSG přidružená tooany podsíť, spusťte hello `azure network vnet subnet set` jak je znázorněno v [zrušit přidružení skupiny NSG z podsítě](#Dissociate-an-NSG-from-a-subnet) pro každou podsíť.
4. hello toodelete NSG, spusťte následující příkaz hello:

    ```azurecli
    az network nsg delete --resource-group RG-NSG --name NSG-FrontEnd
    ```
## <a name="next-steps"></a>Další kroky
* [Povolit protokolování](virtual-network-nsg-manage-log.md) pro skupiny Nsg.

