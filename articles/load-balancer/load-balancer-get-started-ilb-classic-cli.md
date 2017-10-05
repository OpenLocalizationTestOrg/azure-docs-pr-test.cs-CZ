---
title: "Vytvoření interního nástroje pro vyrovnávání zatížení – klasický příkazový řádek Azure CLI | Dokumentace Microsoftu"
description: "Zjistěte, jak vytvořit interní nástroj pro vyrovnávání zatížení pomocí rozhraní příkazového řádku Azure v modelu nasazení Classic"
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
ms.openlocfilehash: d24b95f75b5ffd1116b07cf9f8bac33767a9c835
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-creating-an-internal-load-balancer-classic-using-the-azure-cli"></a><span data-ttu-id="edb70-103">Začínáme vytvářet interní nástroj pro vyrovnávání zatížení (Classic) pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="edb70-103">Get started creating an internal load balancer (classic) using the Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="edb70-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="edb70-104">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [<span data-ttu-id="edb70-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="edb70-105">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [<span data-ttu-id="edb70-106">Cloudové služby</span><span class="sxs-lookup"><span data-stu-id="edb70-106">Cloud services</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="edb70-107">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="edb70-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="edb70-108">Tento článek se věnuje použití klasického modelu nasazení.</span><span class="sxs-lookup"><span data-stu-id="edb70-108">This article covers using the classic deployment model.</span></span> <span data-ttu-id="edb70-109">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="edb70-109">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="edb70-110">Zjistěte, jak [provést tento postup pomocí modelu Resource Manageru](load-balancer-get-started-ilb-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="edb70-110">Learn how to [perform these steps using the Resource Manager model](load-balancer-get-started-ilb-arm-cli.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="to-create-an-internal-load-balancer-set-for-virtual-machines"></a><span data-ttu-id="edb70-111">Vytvoření sady interního nástroje pro vyrovnávání zatížení pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="edb70-111">To create an internal load balancer set for virtual machines</span></span>

<span data-ttu-id="edb70-112">Pokud chcete vytvořit sadu interního nástroje pro vyrovnávání zatížení a servery, které do ní budou posílat provoz, musíte provést následující:</span><span class="sxs-lookup"><span data-stu-id="edb70-112">To create an internal load balancer set and the servers that will send their traffic to it, you must do the following:</span></span>

1. <span data-ttu-id="edb70-113">Vytvořte instanci interního vyrovnávání zatížení, která bude koncovým bodem příchozího provozu, u kterého se bude vyrovnávat zatížení napříč servery sady s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="edb70-113">Create an instance of Internal Load Balancing that will be the endpoint of incoming traffic to be load balanced across the servers of a load-balanced set.</span></span>
2. <span data-ttu-id="edb70-114">Přidejte koncové body odpovídající virtuálním počítačům, které budou přijímat příchozí provoz.</span><span class="sxs-lookup"><span data-stu-id="edb70-114">Add endpoints corresponding to the virtual machines that will be receiving the incoming traffic.</span></span>
3. <span data-ttu-id="edb70-115">Nakonfigurujte servery, které budou posílat provoz k vyrovnání zatížení, aby posílaly provoz na virtuální IP adresu instance interního vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="edb70-115">Configure the servers that will be sending the traffic to be load balanced to send their traffic to the virtual IP (VIP) address of the Internal Load Balancing instance.</span></span>

## <a name="step-by-step-creating-an-internal-load-balancer-using-cli"></a><span data-ttu-id="edb70-116">Vytvoření interního nástroje pro vyrovnávání zatížení pomocí rozhraní příkazového řádku krok za krokem</span><span class="sxs-lookup"><span data-stu-id="edb70-116">Step by step creating an internal load balancer using CLI</span></span>

<span data-ttu-id="edb70-117">Tento průvodce ukazuje, jak vytvořit interní nástroj pro vyrovnávání zatížení založený na výše uvedeném scénáři.</span><span class="sxs-lookup"><span data-stu-id="edb70-117">This guide shows how to create an internal load balancer based on the scenario above.</span></span>

1. <span data-ttu-id="edb70-118">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, přejděte na téma [Instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md) a postupujte podle pokynů až do chvíle, kdy můžete vybrat svůj účet a předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="edb70-118">If you have never used Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="edb70-119">Spuštěním příkazu **azure config mode** přejděte do režimu Classic, jak vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="edb70-119">Run the **azure config mode** command to switch to classic mode, as shown below.</span></span>

    ```azurecli
    azure config mode asm
    ```

    <span data-ttu-id="edb70-120">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="edb70-120">Expected output:</span></span>

        info:    New mode is asm

## <a name="create-endpoint-and-load-balancer-set"></a><span data-ttu-id="edb70-121">Vytvoření koncového bodu a sady nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="edb70-121">Create endpoint and load balancer set</span></span>

<span data-ttu-id="edb70-122">Tento scénář předpokládá, že máte virtuální počítače DB1 a DB2 v cloudové službě s názvem mytestcloud.</span><span class="sxs-lookup"><span data-stu-id="edb70-122">The scenario assumes the virtual machines "DB1" and "DB2" in a cloud service called "mytestcloud".</span></span> <span data-ttu-id="edb70-123">Oba virtuální počítače používají virtuální síť s názvem testvnet s podsítí subnet-1.</span><span class="sxs-lookup"><span data-stu-id="edb70-123">Both virtual machines are using a virtual network called my "testvnet" with subnet "subnet-1".</span></span>

<span data-ttu-id="edb70-124">Tento průvodce vytvoří sadu interního nástroje pro vyrovnávání zatížení používající port 1433 jako privátní port a port 1433 jako místní port.</span><span class="sxs-lookup"><span data-stu-id="edb70-124">This guide will create an internal load balancer set using port 1433 as private port and 1433 as local port.</span></span>

<span data-ttu-id="edb70-125">Jedná se o běžný scénář, kdy máte virtuální počítače systému SQL na back-endu, který pomocí interního nástroje pro vyrovnávání zatížení zaručuje, že databázové servery nebudou přímo přístupné přes veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="edb70-125">This is a common scenario where you have SQL virtual machines on the back end using an internal load balancer to guarantee the database servers won't be exposed directly using a public IP address.</span></span>

### <a name="step-1"></a><span data-ttu-id="edb70-126">Krok 1</span><span class="sxs-lookup"><span data-stu-id="edb70-126">Step 1</span></span>

<span data-ttu-id="edb70-127">Vytvořte sadu interního nástroje pro vyrovnávání zatížení pomocí příkazu `azure network service internal-load-balancer add`.</span><span class="sxs-lookup"><span data-stu-id="edb70-127">Create an internal load balancer set using `azure network service internal-load-balancer add`.</span></span>

```azurecli
azure service internal-load-balancer add --serviceName mytestcloud --internalLBName ilbset --subnet-name subnet-1 --static-virtualnetwork-ipaddress 192.168.2.7
```

<span data-ttu-id="edb70-128">Další informace získáte pomocí příkazu `azure service internal-load-balancer --help`.</span><span class="sxs-lookup"><span data-stu-id="edb70-128">Check out `azure service internal-load-balancer --help` for more information.</span></span>

<span data-ttu-id="edb70-129">Vlastnosti interního nástroje pro vyrovnávání zatížení můžete zkontrolovat pomocí příkazu `azure service internal-load-balancer list` *název cloudové služby*.</span><span class="sxs-lookup"><span data-stu-id="edb70-129">You can check the internal load balancer properties using the command `azure service internal-load-balancer list` *cloud service name*.</span></span>

<span data-ttu-id="edb70-130">Následuje příklad výstupu:</span><span class="sxs-lookup"><span data-stu-id="edb70-130">Here follows an example of the output:</span></span>

    azure service internal-load-balancer list my-testcloud
    info:    Executing command service internal-load-balancer list
    + Getting cloud service deployment
    data:    Name    Type     SubnetName  StaticVirtualNetworkIPAddress
    data:    ------  -------  ----------  -----------------------------
    data:    ilbset  Private  subnet-1    192.168.2.7
    info:    service internal-load-balancer list command OK


### <a name="step-2"></a><span data-ttu-id="edb70-131">Krok 2</span><span class="sxs-lookup"><span data-stu-id="edb70-131">Step 2</span></span>

<span data-ttu-id="edb70-132">Sadu interního nástroje pro vyrovnávání zatížení konfigurujete při přidání prvního koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="edb70-132">You configure the internal load balancer set when you add the first endpoint.</span></span> <span data-ttu-id="edb70-133">V tomto kroku přidružíte porty koncového bodu, virtuálního počítače a testu k sadě interního nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="edb70-133">You will associate the endpoint, virtual machine and probe port to the internal load balancer set in this step.</span></span>

```azurecli
azure vm endpoint create db1 1433 --local-port 1433 --protocol tcp --probe-port 1433 --probe-protocol tcp --probe-interval 300 --probe-timeout 600 --internal-load-balancer-name ilbset
```

### <a name="step-3"></a><span data-ttu-id="edb70-134">Krok 3</span><span class="sxs-lookup"><span data-stu-id="edb70-134">Step 3</span></span>

<span data-ttu-id="edb70-135">Ověřte konfiguraci nástroje pro vyrovnávání zatížení pomocí příkazu `azure vm show` *název virtuálního počítače*.</span><span class="sxs-lookup"><span data-stu-id="edb70-135">Verify the load balancer configuration using `azure vm show` *virtual machine name*</span></span>

```azurecli
azure vm show DB1
```

<span data-ttu-id="edb70-136">Výstup bude:</span><span class="sxs-lookup"><span data-stu-id="edb70-136">The output will be:</span></span>

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

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a><span data-ttu-id="edb70-137">Vytvoření koncového bodu vzdálené plochy pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="edb70-137">Create a remote desktop endpoint for a virtual machine</span></span>

<span data-ttu-id="edb70-138">Koncový bod vzdálené plochy k přesměrování síťového provozu z veřejného portu na místní port konkrétního virtuálního počítače můžete vytvořit pomocí příkazu `azure vm endpoint create`.</span><span class="sxs-lookup"><span data-stu-id="edb70-138">You can create a remote desktop endpoint to forward network traffic from a public port to a local port for a specific virtual machine using `azure vm endpoint create`.</span></span>

```azurecli
azure vm endpoint create web1 54580 -k 3389
```

## <a name="remove-virtual-machine-from-load-balancer"></a><span data-ttu-id="edb70-139">Odebrání virtuálního počítače z nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="edb70-139">Remove virtual machine from load balancer</span></span>

<span data-ttu-id="edb70-140">Odebrat virtuální počítač ze sady interního nástroje pro vyrovnávání zatížení můžete odstraněním přidruženého koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="edb70-140">You can remove a virtual machine from an internal load balancer set by deleting the associated endpoint.</span></span> <span data-ttu-id="edb70-141">Po odebrání koncového bodu již virtuální počítač nebude patřit do sady nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="edb70-141">Once the endpoint is removed, the virtual machine won't belong to the load balancer set anymore.</span></span>

<span data-ttu-id="edb70-142">Když použijeme výše uvedený příklad, koncový bod vytvořený pro virtuální počítač DB1 můžete odebrat z interního nástroje pro vyrovnávání zatížení ilbset pomocí příkazu `azure vm endpoint delete`.</span><span class="sxs-lookup"><span data-stu-id="edb70-142">Using the example above, you can remove the endpoint created for virtual machine "DB1" from internal load balancer "ilbset" by using the command `azure vm endpoint delete`.</span></span>

```azurecli
azure vm endpoint delete DB1 tcp-1433-1433
```

<span data-ttu-id="edb70-143">Další informace získáte pomocí příkazu `azure vm endpoint --help`.</span><span class="sxs-lookup"><span data-stu-id="edb70-143">Check out `azure vm endpoint --help` for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="edb70-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="edb70-144">Next steps</span></span>

[<span data-ttu-id="edb70-145">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení pomocí spřažení se zdrojovou IP adresou</span><span class="sxs-lookup"><span data-stu-id="edb70-145">Configure a load balancer distribution mode using source IP affinity</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="edb70-146">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="edb70-146">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
