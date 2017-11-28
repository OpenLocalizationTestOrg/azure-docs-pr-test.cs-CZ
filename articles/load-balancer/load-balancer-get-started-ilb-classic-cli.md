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
# <a name="get-started-creating-an-internal-load-balancer-classic-using-hello-azure-cli"></a><span data-ttu-id="25f28-103">Začínáme interní zařízení na Vyrovnávání zatížení (klasické) pomocí rozhraní příkazového řádku Azure hello</span><span class="sxs-lookup"><span data-stu-id="25f28-103">Get started creating an internal load balancer (classic) using hello Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="25f28-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="25f28-104">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [<span data-ttu-id="25f28-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="25f28-105">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [<span data-ttu-id="25f28-106">Cloudové služby</span><span class="sxs-lookup"><span data-stu-id="25f28-106">Cloud services</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="25f28-107">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="25f28-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="25f28-108">Tento článek se zabývá pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="25f28-108">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="25f28-109">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="25f28-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="25f28-110">Zjistěte, jak příliš[proveďte tyto kroky, pomocí modelu Resource Manager hello](load-balancer-get-started-ilb-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="25f28-110">Learn how too[perform these steps using hello Resource Manager model](load-balancer-get-started-ilb-arm-cli.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="toocreate-an-internal-load-balancer-set-for-virtual-machines"></a><span data-ttu-id="25f28-111">toocreate k interní s vyrovnáváním zatížení pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="25f28-111">toocreate an internal load balancer set for virtual machines</span></span>

<span data-ttu-id="25f28-112">toocreate interní nástroj nastavit a hello servery, které se odesílají tooit jejich přenosy, musíte udělat následující hello:</span><span class="sxs-lookup"><span data-stu-id="25f28-112">toocreate an internal load balancer set and hello servers that will send their traffic tooit, you must do hello following:</span></span>

1. <span data-ttu-id="25f28-113">Vytvoření instance interní Vyrovnávání zatížení, bude koncový bod hello příchozí provoz toobe vyrovnáváno zatížení napříč servery hello sady Vyrovnávání zatížení sítě.</span><span class="sxs-lookup"><span data-stu-id="25f28-113">Create an instance of Internal Load Balancing that will be hello endpoint of incoming traffic toobe load balanced across hello servers of a load-balanced set.</span></span>
2. <span data-ttu-id="25f28-114">Přidáte koncové body odpovídající toohello virtuálních počítačů, které bude moci přijmout příchozí provoz hello.</span><span class="sxs-lookup"><span data-stu-id="25f28-114">Add endpoints corresponding toohello virtual machines that will be receiving hello incoming traffic.</span></span>
3. <span data-ttu-id="25f28-115">Konfiguraci hello serverů, které se budou odesílat, že hello provoz toobe s vyrovnáváním zatížení se toosend jejich provoz toohello virtuální adresa IP (VIP) instance hello interní Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="25f28-115">Configure hello servers that will be sending hello traffic toobe load balanced toosend their traffic toohello virtual IP (VIP) address of hello Internal Load Balancing instance.</span></span>

## <a name="step-by-step-creating-an-internal-load-balancer-using-cli"></a><span data-ttu-id="25f28-116">Vytvoření interního nástroje pro vyrovnávání zatížení pomocí rozhraní příkazového řádku krok za krokem</span><span class="sxs-lookup"><span data-stu-id="25f28-116">Step by step creating an internal load balancer using CLI</span></span>

<span data-ttu-id="25f28-117">Tato příručka ukazuje, jak toocreate interní nástroj na základě výše uvedené hello scénář.</span><span class="sxs-lookup"><span data-stu-id="25f28-117">This guide shows how toocreate an internal load balancer based on hello scenario above.</span></span>

1. <span data-ttu-id="25f28-118">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) a postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.</span><span class="sxs-lookup"><span data-stu-id="25f28-118">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="25f28-119">Spustit hello **azure konfigurace režim** příkaz tooswitch tooclassic režimu, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="25f28-119">Run hello **azure config mode** command tooswitch tooclassic mode, as shown below.</span></span>

    ```azurecli
    azure config mode asm
    ```

    <span data-ttu-id="25f28-120">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="25f28-120">Expected output:</span></span>

        info:    New mode is asm

## <a name="create-endpoint-and-load-balancer-set"></a><span data-ttu-id="25f28-121">Vytvoření koncového bodu a sady nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="25f28-121">Create endpoint and load balancer set</span></span>

<span data-ttu-id="25f28-122">Hello scénář předpokládá hello virtuálních počítačů "DB1" a "DB2" v cloudové službě názvem "mytestcloud".</span><span class="sxs-lookup"><span data-stu-id="25f28-122">hello scenario assumes hello virtual machines "DB1" and "DB2" in a cloud service called "mytestcloud".</span></span> <span data-ttu-id="25f28-123">Oba virtuální počítače používají virtuální síť s názvem testvnet s podsítí subnet-1.</span><span class="sxs-lookup"><span data-stu-id="25f28-123">Both virtual machines are using a virtual network called my "testvnet" with subnet "subnet-1".</span></span>

<span data-ttu-id="25f28-124">Tento průvodce vytvoří sadu interního nástroje pro vyrovnávání zatížení používající port 1433 jako privátní port a port 1433 jako místní port.</span><span class="sxs-lookup"><span data-stu-id="25f28-124">This guide will create an internal load balancer set using port 1433 as private port and 1433 as local port.</span></span>

<span data-ttu-id="25f28-125">Toto je běžný scénář, kdy máte SQL virtuální počítače na hello pomocí back-end, které interní služby load vyrovnávání tooguarantee hello databázové servery nebude zveřejněné přímo pomocí veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="25f28-125">This is a common scenario where you have SQL virtual machines on hello back end using an internal load balancer tooguarantee hello database servers won't be exposed directly using a public IP address.</span></span>

### <a name="step-1"></a><span data-ttu-id="25f28-126">Krok 1</span><span class="sxs-lookup"><span data-stu-id="25f28-126">Step 1</span></span>

<span data-ttu-id="25f28-127">Vytvořte sadu interního nástroje pro vyrovnávání zatížení pomocí příkazu `azure network service internal-load-balancer add`.</span><span class="sxs-lookup"><span data-stu-id="25f28-127">Create an internal load balancer set using `azure network service internal-load-balancer add`.</span></span>

```azurecli
azure service internal-load-balancer add --serviceName mytestcloud --internalLBName ilbset --subnet-name subnet-1 --static-virtualnetwork-ipaddress 192.168.2.7
```

<span data-ttu-id="25f28-128">Další informace získáte pomocí příkazu `azure service internal-load-balancer --help`.</span><span class="sxs-lookup"><span data-stu-id="25f28-128">Check out `azure service internal-load-balancer --help` for more information.</span></span>

<span data-ttu-id="25f28-129">Můžete zkontrolovat vlastnosti služby vyrovnání zatížení interní hello pomocí příkazu hello `azure service internal-load-balancer list` *název cloudové služby*.</span><span class="sxs-lookup"><span data-stu-id="25f28-129">You can check hello internal load balancer properties using hello command `azure service internal-load-balancer list` *cloud service name*.</span></span>

<span data-ttu-id="25f28-130">Zde následuje příklad výstupu hello:</span><span class="sxs-lookup"><span data-stu-id="25f28-130">Here follows an example of hello output:</span></span>

    azure service internal-load-balancer list my-testcloud
    info:    Executing command service internal-load-balancer list
    + Getting cloud service deployment
    data:    Name    Type     SubnetName  StaticVirtualNetworkIPAddress
    data:    ------  -------  ----------  -----------------------------
    data:    ilbset  Private  subnet-1    192.168.2.7
    info:    service internal-load-balancer list command OK


### <a name="step-2"></a><span data-ttu-id="25f28-131">Krok 2</span><span class="sxs-lookup"><span data-stu-id="25f28-131">Step 2</span></span>

<span data-ttu-id="25f28-132">Můžete nakonfigurovat hello interní s vyrovnáváním zatížení při přidání hello první koncový bod.</span><span class="sxs-lookup"><span data-stu-id="25f28-132">You configure hello internal load balancer set when you add hello first endpoint.</span></span> <span data-ttu-id="25f28-133">Přidružíte hello koncový bod, virtuální počítač a kontroly portu toohello interní s vyrovnáváním zatížení v tomto kroku.</span><span class="sxs-lookup"><span data-stu-id="25f28-133">You will associate hello endpoint, virtual machine and probe port toohello internal load balancer set in this step.</span></span>

```azurecli
azure vm endpoint create db1 1433 --local-port 1433 --protocol tcp --probe-port 1433 --probe-protocol tcp --probe-interval 300 --probe-timeout 600 --internal-load-balancer-name ilbset
```

### <a name="step-3"></a><span data-ttu-id="25f28-134">Krok 3</span><span class="sxs-lookup"><span data-stu-id="25f28-134">Step 3</span></span>

<span data-ttu-id="25f28-135">Ověření konfigurace služby Vyrovnávání zatížení hello pomocí `azure vm show` *název virtuálního počítače*</span><span class="sxs-lookup"><span data-stu-id="25f28-135">Verify hello load balancer configuration using `azure vm show` *virtual machine name*</span></span>

```azurecli
azure vm show DB1
```

<span data-ttu-id="25f28-136">výstup Hello bude:</span><span class="sxs-lookup"><span data-stu-id="25f28-136">hello output will be:</span></span>

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

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a><span data-ttu-id="25f28-137">Vytvoření koncového bodu vzdálené plochy pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="25f28-137">Create a remote desktop endpoint for a virtual machine</span></span>

<span data-ttu-id="25f28-138">Můžete vytvořit vzdálené ploše koncový bod tooforward síťový provoz z místního portu veřejný port tooa pro konkrétní virtuální počítač pomocí `azure vm endpoint create`.</span><span class="sxs-lookup"><span data-stu-id="25f28-138">You can create a remote desktop endpoint tooforward network traffic from a public port tooa local port for a specific virtual machine using `azure vm endpoint create`.</span></span>

```azurecli
azure vm endpoint create web1 54580 -k 3389
```

## <a name="remove-virtual-machine-from-load-balancer"></a><span data-ttu-id="25f28-139">Odebrání virtuálního počítače z nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="25f28-139">Remove virtual machine from load balancer</span></span>

<span data-ttu-id="25f28-140">Virtuální počítač můžete odebrat ze interní s vyrovnáváním zatížení odstraněním hello související koncový bod.</span><span class="sxs-lookup"><span data-stu-id="25f28-140">You can remove a virtual machine from an internal load balancer set by deleting hello associated endpoint.</span></span> <span data-ttu-id="25f28-141">Odebraný hello koncový bod nebude toohello s vyrovnáváním zatížení už patří hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="25f28-141">Once hello endpoint is removed, hello virtual machine won't belong toohello load balancer set anymore.</span></span>

<span data-ttu-id="25f28-142">Pomocí výše uvedeném příkladu hello, můžete odebrat hello koncového bodu pro virtuální počítač "DB1" vytvořit z nástroje pro vyrovnávání zatížení pro vnitřní "ilbset" hello příkazem `azure vm endpoint delete`.</span><span class="sxs-lookup"><span data-stu-id="25f28-142">Using hello example above, you can remove hello endpoint created for virtual machine "DB1" from internal load balancer "ilbset" by using hello command `azure vm endpoint delete`.</span></span>

```azurecli
azure vm endpoint delete DB1 tcp-1433-1433
```

<span data-ttu-id="25f28-143">Další informace získáte pomocí příkazu `azure vm endpoint --help`.</span><span class="sxs-lookup"><span data-stu-id="25f28-143">Check out `azure vm endpoint --help` for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="25f28-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="25f28-144">Next steps</span></span>

[<span data-ttu-id="25f28-145">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení pomocí spřažení se zdrojovou IP adresou</span><span class="sxs-lookup"><span data-stu-id="25f28-145">Configure a load balancer distribution mode using source IP affinity</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="25f28-146">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="25f28-146">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
