---
title: "aaaCreate skupin zabezpečení - sítě, Azure CLI 2.0 | Microsoft Docs"
description: "Zjistěte, jak toocreate a nasazení skupin zabezpečení sítě pomocí hello 2.0 rozhraní příkazového řádku Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9ea82c09-f4a6-4268-88bc-fc439db40c48
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/17/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 30b1d60676331bf5e2bbbb046c747477be9d3338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-network-security-groups-using-hello-azure-cli-20"></a>Vytvořit síť pomocí Azure CLI 2.0 hello skupin zabezpečení

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku 

Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku: 

- [Azure CLI 1.0](virtual-networks-create-nsg-cli-nodejs.md) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modely 
- [Azure CLI 2.0](#Create-the-nsg-for-the-front-end-subnet) -naší nové generace rozhraní příkazového řádku pro model nasazení prostředků správu hello (v tomto článku)

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Ukázka Hello Azure CLI 2.0 příkazy následující očekávat jednoduché prostředí již vytvořili závislosti na scénáři hello předchozí. 

## <a name="create-hello-nsg-for-hello-frontend-subnet"></a>Vytvoření hello skupina NSG pro hello `FrontEnd` podsítě

toocreate skupinu NSG s názvem *NSG front-endu* podle hello scénář předchozí, postupujte podle následujících kroků hello.

1. Pokud nebyly dosud, nainstalujete a nakonfigurujete hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login). 

2. Vytvořit skupinu NSG pomocí hello [vytvořit az sítě nsg](/cli/azure/network/nsg#create) příkaz. 

    ```azurecli
    az network nsg create \
    --resource-group testrg \
    --name NSG-FrontEnd \
    --location centralus 
    ```

    Parametry:
   
   * `--resource-group`: Název skupiny prostředků hello, kde se má vytvořit hello NSG. V našem scénáři je to *TestRG*.
   * `--location`: Oblast azure, kde hello nová skupina NSG je vytvořena. Pro náš scénář *westus*.
   * `--name`: Název hello nová skupina NSG. Pro náš scénář *NSG front-endu*.

    Hello očekává, že výstup je poměrně bit informace včetně seznam všech pravidel výchozí hello. Hello následující příklad ukazuje hello výchozí pravidla pomocí filtru dotazu JMESPATH hello `table` výstupní formát:

    ```azurecli
    az network nsg show \
    -g testrg \
    -n nsg-frontend \
    --query 'defaultSecurityRules[].{Access:access,Desc:description,DestPortRange:destinationPortRange,Direction:direction,Priority:priority}' \
    -o table
    ```
   
   Výstup:

        Access    Desc                                                    DestPortRange    Direction      Priority
        
        Allow     Allow inbound traffic from all VMs in VNET              *                Inbound           65000
        Allow     Allow inbound traffic from azure load balancer          *                Inbound           65001
        Deny      Deny all inbound traffic                                *                Inbound           65500
        Allow     Allow outbound traffic from all VMs tooall VMs in VNET  *                Outbound          65000
        Allow     Allow outbound traffic from all VMs tooInternet         *                Outbound          65001
        Deny      Deny all outbound traffic                               *                Outbound          65500



3. Vytvořte pravidlo, které umožňuje přístup tooport 3389 (RDP) z hello Internet s hello [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create) příkaz.

    > [!NOTE]
    > V závislosti na prostředí hello používáte, může být nutné toomodify hello `*` znak v argumentech hello následující tak, aby nebyla tooexpand hello argument před spuštěním.
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-FrontEnd \
    --name rdp-rule \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 100 \
    --source-address-prefix Internet \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range 3389
    ```
   
    Očekávaný výstup:
   
    ```json
    {
        "access": "Allow",
        "description": null,
        "destinationAddressPrefix": "*",
        "destinationPortRange": "3389",
        "direction": "Inbound",
        "etag": "W/\"<guid>\"",
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule",
        "name": "rdp-rule",
        "priority": 100,
        "protocol": "Tcp",
        "provisioningState": "Succeeded",
        "resourceGroup": "testrg",
        "sourceAddressPrefix": "Internet",
        "sourcePortRange": "*"
    }
    ```

    Parametry:

    * `--resource-group testrg`: hello toouse skupiny prostředků. Všimněte si, že se jedná o velká a malá písmena.
    * `--nsg-name NSG-FrontEnd`: Název hello NSG, ve které hello bude vytvořeno pravidlo.
    * `--name rdp-rule`: Název pro nové pravidlo hello.
    * `--access Allow`: Úroveň přístupu pro pravidlo hello (zakázat nebo povolit).
    * `--protocol Tcp`: Protokol (Tcp, Udp nebo *).
    * `--direction Inbound`: Směr hello připojení (příchozí nebo odchozí).
    * `--priority 100`: Prioritu pro pravidlo hello.
    * `--source-address-prefix Internet`: Předpona zdrojové adresy v CIDR nebo pomocí výchozí značky.
    * `--source-port-range "*"`: Zdrojové portu nebo rozsah portů. Port, který otevírá hello připojení.
    * `--destination-address-prefix "*"`: Předpona cílové adresy v CIDR nebo pomocí výchozí značky.
    * `--destination-port-range 3389`: Cílový port nebo rozsah portů. Port, který obdrží požadavek na připojení hello.



4. Vytvořte pravidlo, které umožňuje přístup tooport 80 (HTTP) z hello Internet **vytvořit pravidla nsg sítě az** příkaz.
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-FrontEnd \
    --name web-rule \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 200 \
    --source-address-prefix Internet \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range 80
    ```
   
    Očekávaný putput:
   
    ```json
    {
        "access": "Allow",
        "description": null,
        "destinationAddressPrefix": "*",
        "destinationPortRange": "80",
        "direction": "Inbound",
        "etag": "W/\"<guid>\"",
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule",
        "name": "web-rule",
        "priority": 200,
        "protocol": "Tcp",
        "provisioningState": "Succeeded",
        "resourceGroup": "testrg",
        "sourceAddressPrefix": "Internet",
        "sourcePortRange": "*"
    }
    ```

5. Vazby hello NSG toohello **front-endu** podsíť s hello [aktualizace az sítě vnet podsíť](/cli/azure/network/vnet/subnet#update) příkaz.
        
    ```azurecli
    az network vnet subnet update \
    --vnet-name TestVNET \
    --name FrontEnd \
    --resource-group testrg \
    --network-security-group NSG-FrontEnd
    ```
   
    Očekávaný výstup:
   
    ```json
    {
        "addressPrefix": "192.168.1.0/24",
        "etag": "W/\"<guid>\"",
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/TestVNET/subnets/FrontEnd",
        "ipConfigurations": [
            {
            "etag": null,
            "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
            "name": null,
            "privateIpAddress": null,
            "privateIpAllocationMethod": null,
            "provisioningState": null,
            "publicIpAddress": null,
            "resourceGroup": "TestRG",
            "subnet": null
            }
        ],
        "name": "FrontEnd",
        "networkSecurityGroup": {
            "defaultSecurityRules": null,
            "etag": null,
            "id": "/subscriptions/<guid>f/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd",
            "location": null,
            "name": null,
            "networkInterfaces": null,
            "provisioningState": null,
            "resourceGroup": "testrg",
            "resourceGuid": null,
            "securityRules": null,
            "subnets": null,
            "tags": null,
            "type": null
        },
        "provisioningState": "Succeeded",
        "resourceGroup": "testrg",
        "resourceNavigationLinks": null,
        "routeTable": null
    }
    ```

## <a name="create-hello-nsg-for-hello-backend-subnet"></a>Vytvoření hello skupina NSG pro hello `BackEnd` podsítě
toocreate skupinu NSG s názvem *NSG back-end* podle hello scénář předchozí, postupujte podle následujících kroků hello.

1. Vytvoření hello `NSG-BackEnd` NSG s **vytvořit az sítě nsg**.
   
    ```azurecli
    az network nsg create \
    --resource-group testrg \
    --name NSG-BackEnd \
    --location centralus
    ```
   
    Jako v kroku 2, předchozí hello očekává, že výstup je poměrně rozsáhlé, včetně výchozí pravidla.
   
2. Vytvořte pravidlo, které umožňuje přístup tooport 1433 (SQL) z hello `FrontEnd` podsíť s hello **vytvořit pravidla nsg sítě az** příkaz.
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-BackEnd \
    --name sql-rule \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 100 \
    --source-address-prefix 192.168.1.0/24 \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range 1433
    ```
   
    Očekávaný výstup:

    ```json  
    {
    "access": "Allow",
    "description": null,
    "destinationAddressPrefix": "*",
    "destinationPortRange": "1433",
    "direction": "Inbound",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd/securityRules/sql-rule",
    "name": "sql-rule",
    "priority": 100,
    "protocol": "Tcp",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "sourceAddressPrefix": "192.168.1.0/24",
    "sourcePortRange": "*"
    }
    ```

3. Vytvořte pravidlo, které odepře přístup toohello Internetu pomocí hello **vytvořit pravidla nsg sítě az** příkaz.
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-BackEnd \
    --name web-rule \
    --access Deny \
    --protocol Tcp  \
    --direction Outbound  \
    --priority 200 \
    --source-address-prefix "*" \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range "*"
    ```
   
    Očekávaný putput:
   
    ```json
    {
    "access": "Deny",
    "description": null,
    "destinationAddressPrefix": "*",
    "destinationPortRange": "*",
    "direction": "Outbound",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd/securityRules/web-rule",
    "name": "web-rule",
    "priority": 200,
    "protocol": "Tcp",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "sourceAddressPrefix": "*",
    "sourcePortRange": "*"
    }
    ```

4. Vazby hello NSG toohello `BackEnd` podsíť pomocí zápisu hello **az sítě vnet podsíť sadu** příkaz.
   
    ```azurecli
    az network vnet subnet update \
    --vnet-name TestVNET \
    --name BackEnd \
    --resource-group testrg \
    --network-security-group NSG-BackEnd
    ```
   
    Očekávaný výstup:
   
    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/TestVNET/subnets/BackEnd",
    "ipConfigurations": null,
    "name": "BackEnd",
    "networkSecurityGroup": {
        "defaultSecurityRules": null,
        "etag": null,
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd",
        "location": null,
        "name": null,
        "networkInterfaces": null,
        "provisioningState": null,
        "resourceGroup": "testrg",
        "resourceGuid": null,
        "securityRules": null,
        "subnets": null,
        "tags": null,
        "type": null
    },
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "resourceNavigationLinks": null,
    "routeTable": null
    }
    ```
