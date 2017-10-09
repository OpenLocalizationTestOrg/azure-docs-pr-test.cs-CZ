---
title: "aaaCreate virtuální síť – 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocreate virtuální sítě pomocí Azure CLI 2.0."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 75966bcc-0056-4667-8482-6f08ca38e77a
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e79b7fe780fc81f4866f810d830824e43a5a43b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-using-hello-azure-cli-20"></a>Vytvoření virtuální sítě pomocí hello 2.0 rozhraní příkazového řádku Azure

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Azure nabízí dva modely nasazení: Azure Resource Manager a Classic. Společnost Microsoft doporučuje vytváření prostředků prostřednictvím modelu nasazení Resource Manager hello. Další informace o toolearn hello rozdíly mezi hello dva modely, přečtěte si hello [modelech nasazení Azure pochopit](../azure-resource-manager/resource-manager-deployment-model.md) článku.

## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku
Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

- [Azure CLI 1.0](virtual-networks-create-vnet-cli-nodejs.md) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modely
- [Azure CLI 2.0](#create-a-virtual-network) -naší nové generace rozhraní příkazového řádku pro model nasazení prostředků správu hello (v tomto článku).
 
    Můžete také vytvořit virtuální síť pomocí Resource Manager pomocí jiných nástrojů nebo vytvoření virtuální sítě pomocí modelu nasazení classic hello výběrem jinou možnost z hello následující seznamu:

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-networks-create-vnet-arm-pportal.md)
> * [PowerShell](virtual-networks-create-vnet-arm-ps.md)
> * [Rozhraní příkazového řádku](virtual-networks-create-vnet-arm-cli.md)
> * [Šablona](virtual-networks-create-vnet-arm-template-click.md)
> * [Portál (Classic)](virtual-networks-create-vnet-classic-pportal.md)
> * [PowerShell (Classic)](virtual-networks-create-vnet-classic-netcfg-ps.md)
> * [Rozhraní příkazového řádku (Classic)](virtual-networks-create-vnet-classic-cli.md)

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]


## <a name="create-a-virtual-network"></a>Vytvoření virtuální sítě

virtuální síť pomocí toocreate hello Azure CLI 2.0, dokončení hello následující kroky:

1. Instalace a konfigurace hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).

2. Vytvořte skupinu prostředků pro virtuální síť pomocí hello [vytvořit skupinu az](/cli/azure/group#create) s hello `--name` a `--location` argumenty:

    ```azurecli
    az group create --name TestRG --location centralus
    ```

3. Vytvoření virtuální sítě a podsítě:

    ```azurecli
    az network vnet create \
    --name TestVNet \
    --resource-group TestRG \
    --location centralus \
    --address-prefix 192.168.0.0/16 \
    --subnet-name FrontEnd \
    --subnet-prefix 192.168.1.0/24
    ```

    Očekávaný výstup:
    
    ```json
    {
        "newVNet": {
            "addressSpace": {
            "addressPrefixes": [
            "192.168.0.0/16"
            ]
            },
            "dhcpOptions": {
            "dnsServers": []
            },
            "provisioningState": "Succeeded",
            "resourceGuid": "<guid>",
            "subnets": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                "name": "FrontEnd",
                "properties": {
                "addressPrefix": "192.168.1.0/24",
                "provisioningState": "Succeeded"
                },
                "resourceGroup": "TestRG"
            }
            ]
            }
    }
    ```

    Použité parametry:

    - `--name TestVNet`: Název toobe hello virtuální síť vytvořili.
    - `--resource-group TestRG`: # název skupiny prostředků v hello, kterými se řídí hello prostředků. 
    - `--location centralus`: hello umístění, do které toodeploy.
    - `--address-prefix 192.168.0.0/16`: hello předponu adresy a bloku.  
    - `--subnet-name FrontEnd`: název hello hello podsítě.
    - `--subnet-prefix 192.168.1.0/24`: hello předponu adresy a bloku.

    toolist hello základní informace toouse v hello vedle příkazů, můžete dotazovat pomocí virtuální sítě hello [filtr dotazu](/cli/azure/query-az-cli2):

    ```azurecli
    az network vnet list --query '[?name==`TestVNet`].{Where:location,Name:name,Group:resourceGroup}' -o table
    ```

    Produkuje hello následující výstup:

        Where      Name      Group

        centralus  TestVNet  TestRG

4. Vytvoření podsítě:

    ```azurecli
    az network vnet subnet create \
    --address-prefix 192.168.2.0/24 \
    --name BackEnd \
    --resource-group TestRG \
    --vnet-name TestVNet
    ```

    Očekávaný výstup:

    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid> \"",
    "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
    "ipConfigurations": null,
    "name": "BackEnd",
    "networkSecurityGroup": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "TestRG",
    "resourceNavigationLinks": null,
    "routeTable": null
    }
    ```

    Použité parametry:

    - `--address-prefix 192.168.2.0/24`: Blok CIDR podsítě.
    - `--name BackEnd`: Název hello novou podsíť.
    - `--resource-group TestRG`: skupinu prostředků hello.
    - `--vnet-name TestVNet`: název hello hello vlastnící virtuální sítě.

5. Vlastnosti hello dotazu hello nové virtuální sítě:

    ```azurecli
    az network vnet show \
    -g TestRG \
    -n TestVNet \
    --query '{Name:name,Where:location,Group:resourceGroup,Status:provisioningState,SubnetCount:subnets | length(@)}' \
    -o table
    ```

    Očekávaný výstup:

        Name      Where      Group    Status       SubnetCount

        TestVNet  centralus  TestRG   Succeeded              2

6. Dotaz hello vlastnosti hello podsítě:

    ```azurecli
    az network vnet subnet list \
    -g TestRG \
    --vnet-name testvnet \
    --query '[].{Name:name,CIDR:addressPrefix,Status:provisioningState}' \
    -o table
    ```

    Očekávaný výstup:

        Name      CIDR            Status

        FrontEnd  192.168.1.0/24  Succeeded
        BackEnd   192.168.2.0/24  Succeeded

## <a name="next-steps"></a>Další kroky

Zjistěte, jak tooconnect:

- Virtuální síť virtuálních počítačů (VM) tooa načtením hello [vytvoření virtuálního počítače s Linuxem](../virtual-machines/linux/quick-create-cli.md) článku. Místo vytváření virtuálních sítí a podsítí v krocích hello hello článků, můžete vybrat z existující virtuální síť a podsíť tooconnect virtuální počítač, abyste.
- Hello virtuální sítě tooother virtuální sítě načtením hello [připojení virtuální sítě](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) článku.
- Hello virtuální sítě tooan do místní sítě pomocí virtuální privátní sítě site-to-site (VPN) nebo okruh ExpressRoute. Zjistěte, jak načtením hello [připojit místní sítě tooan virtuální sítě pomocí sítě site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) a [propojení virtuální sítě tooan okruh ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).
