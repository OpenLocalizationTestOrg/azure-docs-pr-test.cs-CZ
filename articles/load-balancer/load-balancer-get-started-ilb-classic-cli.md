---
title: "aaaCreate interní nástroj pro vyrovnávání - zatížení, rozhraní příkazového řádku Azure classic | Microsoft Docs"
description: "Zjistěte, jak hello toocreate nástroje pro vyrovnávání zatížení pro vnitřní pomocí rozhraní příkazového řádku Azure v modelu nasazení classic hello"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: becbbbde-a118-4269-9444-d3153f00bf34
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: ef29dfda5f7a75a411bbabe8b688a31c6bf81113
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internal-load-balancer-classic-using-hello-azure-cli"></a>Začínáme interní zařízení na Vyrovnávání zatížení (klasické) pomocí rozhraní příkazového řádku Azure hello

> [!div class="op_single_selector"]
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [Cloudové služby](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).  Tento článek se zabývá pomocí modelu nasazení classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Zjistěte, jak příliš[proveďte tyto kroky, pomocí modelu Resource Manager hello](load-balancer-get-started-ilb-arm-cli.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="toocreate-an-internal-load-balancer-set-for-virtual-machines"></a>toocreate k interní s vyrovnáváním zatížení pro virtuální počítače

toocreate interní nástroj nastavit a hello servery, které se odesílají tooit jejich přenosy, musíte udělat následující hello:

1. Vytvoření instance interní Vyrovnávání zatížení, bude koncový bod hello příchozí provoz toobe vyrovnáváno zatížení napříč servery hello sady Vyrovnávání zatížení sítě.
2. Přidáte koncové body odpovídající toohello virtuálních počítačů, které bude moci přijmout příchozí provoz hello.
3. Konfiguraci hello serverů, které se budou odesílat, že hello provoz toobe s vyrovnáváním zatížení se toosend jejich provoz toohello virtuální adresa IP (VIP) instance hello interní Vyrovnávání zatížení.

## <a name="step-by-step-creating-an-internal-load-balancer-using-cli"></a>Vytvoření interního nástroje pro vyrovnávání zatížení pomocí rozhraní příkazového řádku krok za krokem

Tato příručka ukazuje, jak toocreate interní nástroj na základě výše uvedené hello scénář.

1. Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) a postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.
2. Spustit hello **azure konfigurace režim** příkaz tooswitch tooclassic režimu, jak je uvedeno níže.

    ```azurecli
    azure config mode asm
    ```

    Očekávaný výstup:

        info:    New mode is asm

## <a name="create-endpoint-and-load-balancer-set"></a>Vytvoření koncového bodu a sady nástroje pro vyrovnávání zatížení

Hello scénář předpokládá hello virtuálních počítačů "DB1" a "DB2" v cloudové službě názvem "mytestcloud". Oba virtuální počítače používají virtuální síť s názvem testvnet s podsítí subnet-1.

Tento průvodce vytvoří sadu interního nástroje pro vyrovnávání zatížení používající port 1433 jako privátní port a port 1433 jako místní port.

Toto je běžný scénář, kdy máte SQL virtuální počítače na hello pomocí back-end, které interní služby load vyrovnávání tooguarantee hello databázové servery nebude zveřejněné přímo pomocí veřejnou IP adresu.

### <a name="step-1"></a>Krok 1

Vytvořte sadu interního nástroje pro vyrovnávání zatížení pomocí příkazu `azure network service internal-load-balancer add`.

```azurecli
azure service internal-load-balancer add --serviceName mytestcloud --internalLBName ilbset --subnet-name subnet-1 --static-virtualnetwork-ipaddress 192.168.2.7
```

Další informace získáte pomocí příkazu `azure service internal-load-balancer --help`.

Můžete zkontrolovat vlastnosti služby vyrovnání zatížení interní hello pomocí příkazu hello `azure service internal-load-balancer list` *název cloudové služby*.

Zde následuje příklad výstupu hello:

    azure service internal-load-balancer list my-testcloud
    info:    Executing command service internal-load-balancer list
    + Getting cloud service deployment
    data:    Name    Type     SubnetName  StaticVirtualNetworkIPAddress
    data:    ------  -------  ----------  -----------------------------
    data:    ilbset  Private  subnet-1    192.168.2.7
    info:    service internal-load-balancer list command OK


### <a name="step-2"></a>Krok 2

Můžete nakonfigurovat hello interní s vyrovnáváním zatížení při přidání hello první koncový bod. Přidružíte hello koncový bod, virtuální počítač a kontroly portu toohello interní s vyrovnáváním zatížení v tomto kroku.

```azurecli
azure vm endpoint create db1 1433 --local-port 1433 --protocol tcp --probe-port 1433 --probe-protocol tcp --probe-interval 300 --probe-timeout 600 --internal-load-balancer-name ilbset
```

### <a name="step-3"></a>Krok 3

Ověření konfigurace služby Vyrovnávání zatížení hello pomocí `azure vm show` *název virtuálního počítače*

```azurecli
azure vm show DB1
```

výstup Hello bude:

    azure vm show DB1
    info:    Executing command vm show
    + Getting virtual machines
    data:    DNSName "mytestcloud.cloudapp.net"
    data:    Location "East US 2"
    data:    VMName "DB1"
    data:    IPAddress "192.168.2.4"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "db1-DB1-0-201511120457370846"
    data:    OSDisk mediaLink "https://XXXX.blob.core.windows.net/vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "137.116.64.107"
    data:    VirtualIPAddresses 0 name "db1ContractContract"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    VirtualIPAddresses 1 address "192.168.2.7"
    data:    VirtualIPAddresses 1 name "ilbset"
    data:    Network Endpoints 0 localPort 5986
    data:    Network Endpoints 0 name "PowerShell"
    data:    Network Endpoints 0 port 5986
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 3389
    data:    Network Endpoints 1 name "Remote Desktop"
    data:    Network Endpoints 1 port 60173
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 1433
    data:    Network Endpoints 2 name "tcp-1433-1433"
    data:    Network Endpoints 2 port 1433
    data:    Network Endpoints 2 loadBalancerProbe port 1433
    data:    Network Endpoints 2 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 2 loadBalancerProbe intervalInSeconds 300
    data:    Network Endpoints 2 loadBalancerProbe timeoutInSeconds 600
    data:    Network Endpoints 2 protocol "tcp"
    data:    Network Endpoints 2 virtualIPAddress "192.168.2.7"
    data:    Network Endpoints 2 enableDirectServerReturn false
    data:    Network Endpoints 2 loadBalancerName "ilbset"
    info:    vm show command OK

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Vytvoření koncového bodu vzdálené plochy pro virtuální počítač

Můžete vytvořit vzdálené ploše koncový bod tooforward síťový provoz z místního portu veřejný port tooa pro konkrétní virtuální počítač pomocí `azure vm endpoint create`.

```azurecli
azure vm endpoint create web1 54580 -k 3389
```

## <a name="remove-virtual-machine-from-load-balancer"></a>Odebrání virtuálního počítače z nástroje pro vyrovnávání zatížení

Virtuální počítač můžete odebrat ze interní s vyrovnáváním zatížení odstraněním hello související koncový bod. Odebraný hello koncový bod nebude toohello s vyrovnáváním zatížení už patří hello virtuálního počítače.

Pomocí výše uvedeném příkladu hello, můžete odebrat hello koncového bodu pro virtuální počítač "DB1" vytvořit z nástroje pro vyrovnávání zatížení pro vnitřní "ilbset" hello příkazem `azure vm endpoint delete`.

```azurecli
azure vm endpoint delete DB1 tcp-1433-1433
```

Další informace získáte pomocí příkazu `azure vm endpoint --help`.

## <a name="next-steps"></a>Další kroky

[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení pomocí spřažení se zdrojovou IP adresou](load-balancer-distribution-mode.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)
