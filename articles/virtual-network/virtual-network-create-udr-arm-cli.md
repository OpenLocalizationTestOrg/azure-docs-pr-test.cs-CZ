---
title: "Směrování a virtuální zařízení aaaControl pomocí hello 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocontrol směrování a virtuální zařízení pomocí Azure CLI 2.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 5452a0b8-21a6-4699-8d6a-e2d8faf32c25
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/12/2017
ms.author: jdial
ms.openlocfilehash: 79b908848932a4365dab1b7497b6a0dbf44bbaf8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-user-defined-routes-udr-using-hello-azure-cli-20"></a>Vytvoření trasy definované uživatelem (UDR) pomocí hello 2.0 rozhraní příkazového řádku Azure

> [!div class="op_single_selector"]
> * [PowerShell](virtual-network-create-udr-arm-ps.md)
> * [Azure CLI](virtual-network-create-udr-arm-cli.md)
> * [Šablona](virtual-network-create-udr-arm-template.md)
> * [Prostředí PowerShell (nasazení Classic)](virtual-network-create-udr-classic-ps.md)
> * [Rozhraní příkazového řádku (nasazení Classic)](virtual-network-create-udr-classic-cli.md)

## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku 

Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku: 

- [Azure CLI 1.0](virtual-network-create-udr-arm-cli-nodejs.md) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modely 
- [Azure CLI 2.0](#Create-the-UDR-for-the-front-end-subnet) -naší nové generace rozhraní příkazového řádku pro model nasazení prostředků správu hello (v tomto článku)

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

níže uvedené příkazy rozhraní příkazového řádku Azure ukázkové Hello očekávat jednoduché prostředí již vytvořili závislosti na scénáři hello výše. Pokud chcete příkazy hello toorun, jak jsou zobrazeny v tomto dokumentu, vytvoření nasazením nejprve hello testovací prostředí [této šablony](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), klikněte na tlačítko **nasazení tooAzure**, nahraďte hello výchozí hodnoty parametrů Pokud potřeby a postupujte podle pokynů hello v hello portálu.


## <a name="create-hello-udr-for-hello-front-end-subnet"></a>Vytvoření hello UDR podsítě front-endu hello
toocreate hello směrovací tabulku a směrování, které jsou potřebné pro podsítě front end hello podle hello scénář výše, postupujte podle následujících kroků hello.

1. Vytvořit směrovací tabulku pro hello front-end podsíť s hello [vytvoření tabulky trasy sítě az](/cli/azure/network/route-table#create) příkaz:

    ```azurecli
    az network route-table create \
    --resource-group testrg \
    --location centralus \
    --name UDR-FrontEnd
    ```

    Výstup:

    ```json
    {
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd",
    "location": "centralus",
    "name": "UDR-FrontEnd",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "routes": [],
    "subnets": null,
    "tags": null,
    "type": "Microsoft.Network/routeTables"
    }
    ```

2. Vytvoření cesty, která odesílá všechny přenosy určené toohello podsítě back-end (192.168.2.0/24) toohello **FW1** virtuálního počítače (192.168.0.4) pomocí hello [az síťovou směrovací tabulku trasu vytvořit](/cli/azure/network/route-table/route#create) příkaz:

    ```azurecli 
    az network route-table route create \
    --resource-group testrg \
    --name RouteToBackEnd \
    --route-table-name UDR-FrontEnd \
    --address-prefix 192.168.2.0/24 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 192.168.0.4
    ```

    Výstup:

    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd/routes/RouteToBackEnd",
    "name": "RouteToBackEnd",
    "nextHopIpAddress": "192.168.0.4",
    "nextHopType": "VirtualAppliance",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg"
    }
    ```
    Parametry:

    * **– Název směrovací tabulky**. Název hello směrovací tabulka, kam bude přidána hello trasy. Pro náš scénář *UDR front-endu*.
    * **--address prefixes**. Předpona adresy podsítě hello, kde jsou pakety určené do. Pro náš scénář *192.168.2.0/24*.
    * **--Další typ směrování**. Typ objektu provozu se odešle do. Možné hodnoty jsou *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, nebo *žádné*.
    * **--Další směrování ip adresu**. IP adresa dalšího směrování. Pro náš scénář *192.168.0.4*.

3. Spustit hello [aktualizace az sítě vnet podsíť](/cli/azure/network/vnet/subnet#update) příkaz tooassociate hello směrovací tabulku vytvořili výše s hello **front-endu** podsítě:

    ```azurecli
    az network vnet subnet update \
    --resource-group testrg \
    --vnet-name testvnet \
    --name FrontEnd \
    --route-table UDR-FrontEnd
    ```

    Výstup:

    ```json
    {
    "addressPrefix": "192.168.1.0/24",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/FrontEnd",
    "ipConfigurations": null,
    "name": "FrontEnd",
    "networkSecurityGroup": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "resourceNavigationLinks": null,
    "routeTable": {
        "etag": null,
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd",
        "location": null,
        "name": null,
        "provisioningState": null,
        "resourceGroup": "testrg",
        "routes": null,
        "subnets": null,
        "tags": null,
        "type": null
        }
    }
    ```

    Parametry:
    
    * **--vnet-name**. Název hello virtuální síť, kde se nachází hello podsítě. V našem scénáři je to *TestVNet*.

## <a name="create-hello-udr-for-hello-back-end-subnet"></a>Vytvoření hello UDR pro podsíť back-end hello

toocreate hello směrovací tabulky a trasy potřebné pro podsíť back-end hello podle hello scénář výše, dokončení hello následující kroky:

1. Spusťte následující příkaz toocreate hello tabulka směrování pro podsíť back-end hello:

    ```azurecli
    az network route-table create \
    --resource-group testrg \
    --name UDR-BackEnd \
    --location centralus
    ```

2. Spusťte následující příkaz toocreate trasy v toosend tabulky trasy hello hello všechny přenosy určené toohello front-end podsíť (192.168.1.0/24) toohello **FW1** virtuálních počítačů (192.168.0.4):

    ```azurecli
    az network route-table route create \
    --resource-group testrg \
    --name RouteToFrontEnd \
    --route-table-name UDR-BackEnd \
    --address-prefix 192.168.1.0/24 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 192.168.0.4
    ```

3. Spuštění hello následující příkaz tooassociate hello směrovací tabulku s hello **back-end** podsítě:

    ```azurecli
    az network vnet subnet update \
    --resource-group testrg \
    --vnet-name testvnet \
    --name BackEnd \
    --route-table UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-fw1"></a>Povolení předávání IP na FW1

předávání IP tooenable v hello síťový adaptér používá **FW1**, dokončení hello následující kroky:

1. Spustit hello [az sítě seskupování zobrazit](/cli/azure/network/nic#show) s aktuální JMESPATH hello toodisplay filtru **povolení předávání ip** hodnota **předávání IP povolit**. Je potřeba ho nastavit příliš*false*.

    ```azurecli
    az network nic show \
    --resource-group testrg \
    --nname nicfw1 \
    --query 'enableIpForwarding' -o tsv
    ```

    Výstup:

        false

2. Spusťte následující příkaz předávání IP tooenable hello:

    ```azurecli
    az network nic update \
    --resource-group testrg \
    --name nicfw1 \
    --ip-forwarding true
    ```

    Můžete prozkoumat konzoly přenášené datovými proudy toohello výstup hello nebo jenom pro konkrétní hello testování **enableIpForwarding** hodnotu:

    ```azurecli
    az network nic show -g testrg -n nicfw1 --query 'enableIpForwarding' -o tsv
    ```

    Výstup:

        true

    Parametry:

    **předávání--ip**: *true* nebo *false*.

