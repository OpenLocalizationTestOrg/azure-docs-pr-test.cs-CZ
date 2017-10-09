---
title: "virtuální síť pomocí aaaCreate hello 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocreate virtuální sítě pomocí Azure CLI 1.0 | Správce prostředků."
services: virtual-network
documentationcenter: 
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2017
ms.author: jdial
ms.openlocfilehash: f48a8b23a5994164b71c9b51ee8a6810d17f9392
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-using-hello-azure-cli"></a>Vytvoření virtuální sítě pomocí hello rozhraní příkazového řádku Azure

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Azure nabízí dva modely nasazení: Azure Resource Manager a Classic. Společnost Microsoft doporučuje vytváření prostředků prostřednictvím modelu nasazení Resource Manager hello. Další informace o toolearn hello rozdíly mezi hello dva modely, přečtěte si hello [modelech nasazení Azure pochopit](../azure-resource-manager/resource-manager-deployment-model.md) článku.

## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku
Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

- [Azure CLI 2.0](virtual-networks-create-vnet-arm-cli.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello
- [Azure CLI 1.0](#create-a-virtual-network) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)

 
[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a>Vytvoření virtuální sítě

hello toocreate virtuální sítě pomocí rozhraní příkazového řádku Azure, dokončení hello následující kroky:

1. Instalace a konfigurace rozhraní příkazového řádku Azure podle následující hello kroky hello v hello [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) článku.

2. Vytvoření virtuální sítě a podsítě:

    ```azurecli
    azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
    ```

    Očekávaný výstup:
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK

    Použité parametry:

   * **--vnet**. Název toobe hello virtuální síť vytvořili. V našem scénáři je to *TestVNet*.
   * **-e (nebo--adresní prostor)**. Adresní prostor sítě VNet. Pro náš scénář *192.168.0.0*
   * **-i (nebo - cidr)**. Maska sítě ve formátu CIDR. Pro náš scénář *16*.
   * **-n (nebo--název podsítě**). Název první podsíť hello. V našem scénáři je to *FrontEnd*.
   * **-p (nebo--IP adresu podsítě start)**. Počáteční IP adresa pro podsíť nebo adresního prostoru podsítě. Pro náš scénář *192.168.1.0*.
   * **-r (nebo--podsítě cidr)**. Maska sítě ve formátu CIDR podsítě. Pro náš scénář *24*.
   * **-l (nebo --location)**. Oblast Azure, kde se má vytvořit hello virtuální sítě. Pro náš scénář *střed USA*.

3. Vytvoření podsítě:

    ```azurecli
    azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
    ```
   
    Očekávaný výstup:

            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up hello subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK

    Použité parametry:

   * **-t (nebo--vnet-name**. Název hello kde hello se vytvoří podsíť virtuální sítě. V našem scénáři je to *TestVNet*.
   * **-n (nebo --name)**. Název nové podsítě hello. Pro náš scénář *back-end*.
   * **-a (nebo --address-prefixes)**. Blok CIDR podsítě. Čtyři našem scénáři *192.168.2.0/24*.
   
4. Vlastnosti hello tooview hello nové virtuální sítě:

    ```azurecli
    azure network vnet show
    ```
   
    Očekávaný výstup:
   
            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up hello virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK

## <a name="next-steps"></a>Další kroky

Zjistěte, jak tooconnect:

- Virtuální síť virtuálních počítačů (VM) tooa načtením hello [vytvoření virtuálního počítače s Linuxem](../virtual-machines/linux/quick-create-cli.md) článku. Místo vytváření virtuálních sítí a podsítí v krocích hello hello článků, můžete vybrat z existující virtuální síť a podsíť tooconnect virtuální počítač, abyste.
- Hello virtuální sítě tooother virtuální sítě načtením hello [připojení virtuální sítě](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) článku.
- Hello virtuální sítě tooan do místní sítě pomocí virtuální privátní sítě site-to-site (VPN) nebo okruh ExpressRoute. Zjistěte, jak načtením hello [připojit místní sítě tooan virtuální sítě pomocí sítě site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) a [propojení virtuální sítě tooan okruh ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).
