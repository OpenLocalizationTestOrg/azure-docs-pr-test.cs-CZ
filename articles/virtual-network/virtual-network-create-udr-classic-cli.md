---
title: "aaaControl směrování v virtuální síti příkazového - řádku - Azure Classic | Microsoft Docs"
description: "Zjistěte, jak hello toocontrol směrování v virtuální sítě pomocí rozhraní příkazového řádku Azure v modelu nasazení classic hello"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: ca2b4638-8777-4d30-b972-eb790a7c804f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: 07dde573f1a605bf280156c261d51e213ede0cdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="control-routing-and-use-virtual-appliances-classic-using-hello-azure-cli"></a>Ovládací prvek směrování a použití virtuálních zařízení (klasické) pomocí rozhraní příkazového řádku Azure hello

> [!div class="op_single_selector"]
> * [PowerShell](virtual-network-create-udr-arm-ps.md)
> * [Azure CLI](virtual-network-create-udr-arm-cli.md)
> * [Šablona](virtual-network-create-udr-arm-template.md)
> * [PowerShell (Classic)](virtual-network-create-udr-classic-ps.md)
> * [Rozhraní příkazového řádku (Classic)](virtual-network-create-udr-classic-cli.md)

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Tento článek se týká modelu nasazení classic hello. Můžete také [řídit směrování a použití virtuálních zařízení v modelu nasazení Resource Manager hello](virtual-network-create-udr-arm-cli.md).

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

níže uvedené příkazy rozhraní příkazového řádku Azure ukázkové Hello očekávat jednoduché prostředí již vytvořili závislosti na scénáři hello výše. Pokud chcete příkazy hello toorun, jak jsou zobrazeny v tomto dokumentu, vytvoření prostředí hello ukazuje [vytvoření virtuální sítě (klasické) pomocí rozhraní příkazového řádku Azure hello](virtual-networks-create-vnet-classic-cli.md).

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-hello-udr-for-hello-front-end-subnet"></a>Vytvoření hello UDR pro podsítě front end hello
toocreate hello směrovací tabulku a směrování, které jsou potřebné pro podsítě front end hello podle hello scénář výše, postupujte podle následujících kroků hello.

1. Spusťte následující příkaz tooswitch tooclassic režimu hello:

    ```azurecli
    azure config mode asm
    ```

    Výstup:

        info:    New mode is asm

2. Spusťte následující příkaz toocreate hello tabulka směrování pro podsíť pro front-end hello:

    ```azurecli
    azure network route-table create -n UDR-FrontEnd -l uswest
    ```
   
    Výstup:
   
        info:    Executing command network route-table create
        info:    Creating route table "UDR-FrontEnd"
        info:    Getting route table "UDR-FrontEnd"
        data:    Name                            : UDR-FrontEnd
        data:    Location                        : West US
        info:    network route-table create command OK
   
    Parametry:
   
   * **-l (nebo --location)**. Oblast Azure, kde hello nová skupina NSG bude vytvořen. Pro náš scénář *westus*.
   * **-n (nebo --name)**. Název hello nová skupina NSG. Pro náš scénář *NSG front-endu*.
3. Spusťte následující příkaz toocreate trasy v toosend tabulky trasy hello hello všechny přenosy určené toohello podsítě back-end (192.168.2.0/24) toohello **FW1** virtuálních počítačů (192.168.0.4):

    ```azurecli
    azure network route-table route set -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -t VirtualAppliance -p 192.168.0.4
    ```

    Výstup:
   
        info:    Executing command network route-table route set
        info:    Getting route table "UDR-FrontEnd"
        info:    Setting route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    network route-table route set command OK
   
    Parametry:
   
   * **-r (nebo--název směrovací tabulky)**. Název hello směrovací tabulka, kam bude přidána hello trasy. Pro náš scénář *UDR front-endu*.
   * **-a (nebo --address-prefixes)**. Předpona adresy podsítě hello, kde jsou pakety určené do. Pro náš scénář *192.168.2.0/24*.
   * **-t (nebo--další typ směrování)**. Typ objektu provozu se odešle do. Možné hodnoty jsou *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, nebo *žádné*.
   * **-p (nebo--další směrování ip adresu**). IP adresa dalšího směrování. Pro náš scénář *192.168.0.4*.
4. Hello spusťte následující příkaz tooassociate hello směrovací tabulku vytvořené pomocí hello **front-endu** podsítě:

    ```azurecli
    azure network vnet subnet route-table add -t TestVNet -n FrontEnd -r UDR-FrontEnd
    ```
   
    Výstup:
   
        info:    Executing command network vnet subnet route-table add
        info:    Looking up hello subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        info:    Associating route table "UDR-FrontEnd" and subnet "FrontEnd"
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        data:    Route table name                : UDR-FrontEnd
        data:      Location                      : West US
        data:      Routes:
        info:    network vnet subnet route-table add command OK    
   
    Parametry:
   
   * **-t (nebo--vnet-name)**. Název hello virtuální síť, kde se nachází hello podsítě. V našem scénáři je to *TestVNet*.
   * **-n (nebo--název podsítě**. Název hello podsíť hello směrovací tabulka se zařadí do. V našem scénáři je to *FrontEnd*.

## <a name="create-hello-udr-for-hello-back-end-subnet"></a>Vytvoření hello UDR pro podsíť back-end hello
toocreate hello směrovací tabulku a směrování, které jsou potřebné pro podsíť back-end hello podle hello scénáři dokončení hello následující kroky:

1. Spusťte následující příkaz toocreate hello tabulka směrování pro podsíť back-end hello:

    ```azurecli
    azure network route-table create -n UDR-BackEnd -l uswest
    ```

2. Spusťte následující příkaz toocreate trasy v toosend tabulky trasy hello hello všechny přenosy určené toohello front-end podsíť (192.168.1.0/24) toohello **FW1** virtuálních počítačů (192.168.0.4):

    ```azurecli
    azure network route-table route set -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -t VirtualAppliance -p 192.168.0.4
    ```

3. Spuštění hello následující příkaz tooassociate hello směrovací tabulku s hello **back-end** podsítě:

    ```azurecli
    azure network vnet subnet route-table add -t TestVNet -n BackEnd -r UDR-BackEnd
    ```

