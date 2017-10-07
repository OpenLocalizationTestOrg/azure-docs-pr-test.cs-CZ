---
title: "aaaCreate směřujících Internetu pro vyrovnávání zátěže - rozhraní příkazového řádku Azure classic | Microsoft Docs"
description: "Zjistěte, jak toocreate k Internetu přístupných pro vyrovnávání zatížení pomocí modelu nasazení classic hello rozhraní příkazového řádku Azure"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-service-management
ms.assetid: e433a824-4a8a-44d2-8765-a74f52d4e584
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: e6070cbc574f74bca0cccb960ff192847d6511bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-hello-azure-cli"></a>Začínáte s vytvářením internetovým Vyrovnávání zatížení (klasické) v hello rozhraní příkazového řádku Azure

> [!div class="op_single_selector"]
> * [Portál Azure Classic](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [Azure Cloud Services](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> Než začnete pracovat s prostředky Azure, je důležité toounderstand Azure aktuálně má dva modely nasazení: Azure Resource Manager a Klasický model. Před zahájením práce s jakýmikoli prostředky Azure se ujistěte, že rozumíte [modelům nasazení a příslušným nástrojům](../azure-classic-rm.md). Hello dokumentaci k různým nástrojům můžete zobrazit kliknutím na karty hello hello horní části tohoto článku. Tento článek se týká modelu nasazení classic hello. Můžete také [zjistěte, jak toocreate přístupem Internetu pro vyrovnávání zátěže pomocí Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="step-by-step-creating-an-internet-facing-load-balancer-using-cli"></a>Vytvoření internetového nástroje pro vyrovnávání zatížení pomocí rozhraní příkazového řádku krok za krokem

Tato příručka ukazuje, jak toocreate k Internetu pro vyrovnávání zatížení na základě výše uvedené hello scénář.

1. Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) a postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.
2. Spustit hello **azure konfigurace režim** příkaz tooswitch tooclassic režimu, jak je uvedeno níže.

    ```azurecli
    azure config mode asm
    ```

    Očekávaný výstup:

        info:    New mode is asm

## <a name="create-endpoint-and-load-balancer-set"></a>Vytvoření koncového bodu a sady nástroje pro vyrovnávání zatížení

Hello scénář předpokládá hello virtuálních počítačů "web1" a "web2" byly vytvořeny.
Tento průvodce vytvoří sadu nástroje pro vyrovnávání zatížení používající port 80 jako veřejný port a port 80 jako místní port. Port testu je také konfigurován na portu 80 a nástroj pro vyrovnávání zatížení s názvem hello nastavte "lbset".

### <a name="step-1"></a>Krok 1

Vytvoření první koncový bod hello a nastavit pomocí nástroje pro vyrovnání zatížení `azure network vm endpoint create` pro virtuální počítač "web1".

```azurecli
azure vm endpoint create web1 80 --local-port 80 --protocol tcp --probe-port 80 --load-balanced-set-name lbset
```

## <a name="step-2"></a>Krok 2

Přidáte sadu Nástroje pro vyrovnávání zatížení toohello druhý virtuální počítač "webu 2".

```azurecli
azure vm endpoint create web2 80 --local-port 80 --protocol tcp --probe-port 80 --load-balanced-set-name lbset
```

## <a name="step-3"></a>Krok 3

Ověření konfigurace služby Vyrovnávání zatížení hello pomocí `azure vm show` .

```azurecli
azure vm show web1
```

výstup Hello bude:

    data:    DNSName "contoso.cloudapp.net"
    data:    Location "East US"
    data:    VMName "web1"
    data:    IPAddress "10.0.0.5"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-2015
    6-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "joaoma-1-web1-0-201509251804250879"
    data:    OSDisk mediaLink "https://XXXXXXXXXXXXXXX.blob.core.windows.
    /vhds/joaomatest-web1-2015-09-25.vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Se
    r-2012-R2-20150916-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "XXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 name "XXXXXXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    Network Endpoints 0 loadBalancedEndpointSetName "lbset"
    data:    Network Endpoints 0 localPort 80
    data:    Network Endpoints 0 name "tcp-80-80"
    data:    Network Endpoints 0 port 80
    data:    Network Endpoints 0 loadBalancerProbe port 80
    data:    Network Endpoints 0 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 0 loadBalancerProbe intervalInSeconds 15
    data:    Network Endpoints 0 loadBalancerProbe timeoutInSeconds 31
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 5986
    data:    Network Endpoints 1 name "PowerShell"
    data:    Network Endpoints 1 port 5986
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 3389
    data:    Network Endpoints 2 name "Remote Desktop"
    data:    Network Endpoints 2 port 58081
    info:    vm show command OK

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Vytvoření koncového bodu vzdálené plochy pro virtuální počítač

Můžete vytvořit vzdálené ploše koncový bod tooforward síťový provoz z místního portu veřejný port tooa pro konkrétní virtuální počítač pomocí `azure vm endpoint create`.

```azurecli
azure vm endpoint create web1 54580 -k 3389
```

## <a name="remove-virtual-machine-from-load-balancer"></a>Odebrání virtuálního počítače z nástroje pro vyrovnávání zatížení

Máte toodelete hello koncový bod přidružené toohello sady Nástroje pro vyrovnávání zatížení z virtuálního počítače hello. Odebraný hello koncový bod virtuálního počítače hello nepatří toohello s vyrovnáváním zatížení už.

Pomocí výše uvedeném příkladu hello, můžete odebrat hello koncového bodu pro virtuální počítač "web1" vytvořit z nástroje pro vyrovnávání zatížení "lbset" pomocí příkazu hello `azure vm endpoint delete`.

```azurecli
azure vm endpoint delete web1 tcp-80-80
```

> [!NOTE]
> Můžete si prostudovat další koncové body toomanage možnosti pomocí příkazu hello`azure vm endpoint --help`

## <a name="next-steps"></a>Další kroky

[Začínáme s konfigurací interního nástroje pro vyrovnávání zatížení](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)
