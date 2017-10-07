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
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-hello-azure-cli"></a><span data-ttu-id="7c76b-103">Začínáte s vytvářením internetovým Vyrovnávání zatížení (klasické) v hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="7c76b-103">Get started creating an Internet facing load balancer (classic) in hello Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="7c76b-104">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="7c76b-104">Azure classic portal</span></span>](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [<span data-ttu-id="7c76b-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7c76b-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [<span data-ttu-id="7c76b-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7c76b-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [<span data-ttu-id="7c76b-107">Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="7c76b-107">Azure Cloud Services</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="7c76b-108">Než začnete pracovat s prostředky Azure, je důležité toounderstand Azure aktuálně má dva modely nasazení: Azure Resource Manager a Klasický model.</span><span class="sxs-lookup"><span data-stu-id="7c76b-108">Before you work with Azure resources, it's important toounderstand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="7c76b-109">Před zahájením práce s jakýmikoli prostředky Azure se ujistěte, že rozumíte [modelům nasazení a příslušným nástrojům](../azure-classic-rm.md).</span><span class="sxs-lookup"><span data-stu-id="7c76b-109">Make sure you understand [deployment models and tools](../azure-classic-rm.md) before you work with any Azure resource.</span></span> <span data-ttu-id="7c76b-110">Hello dokumentaci k různým nástrojům můžete zobrazit kliknutím na karty hello hello horní části tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="7c76b-110">You can view hello documentation for different tools by clicking hello tabs at hello top of this article.</span></span> <span data-ttu-id="7c76b-111">Tento článek se týká modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="7c76b-111">This article covers hello classic deployment model.</span></span> <span data-ttu-id="7c76b-112">Můžete také [zjistěte, jak toocreate přístupem Internetu pro vyrovnávání zátěže pomocí Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="7c76b-112">You can also [Learn how toocreate an Internet facing load balancer using Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="step-by-step-creating-an-internet-facing-load-balancer-using-cli"></a><span data-ttu-id="7c76b-113">Vytvoření internetového nástroje pro vyrovnávání zatížení pomocí rozhraní příkazového řádku krok za krokem</span><span class="sxs-lookup"><span data-stu-id="7c76b-113">Step by step creating an Internet facing load balancer using CLI</span></span>

<span data-ttu-id="7c76b-114">Tato příručka ukazuje, jak toocreate k Internetu pro vyrovnávání zatížení na základě výše uvedené hello scénář.</span><span class="sxs-lookup"><span data-stu-id="7c76b-114">This guide shows how toocreate an Internet load balancer based on hello scenario above.</span></span>

1. <span data-ttu-id="7c76b-115">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) a postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.</span><span class="sxs-lookup"><span data-stu-id="7c76b-115">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="7c76b-116">Spustit hello **azure konfigurace režim** příkaz tooswitch tooclassic režimu, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="7c76b-116">Run hello **azure config mode** command tooswitch tooclassic mode, as shown below.</span></span>

    ```azurecli
    azure config mode asm
    ```

    <span data-ttu-id="7c76b-117">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="7c76b-117">Expected output:</span></span>

        info:    New mode is asm

## <a name="create-endpoint-and-load-balancer-set"></a><span data-ttu-id="7c76b-118">Vytvoření koncového bodu a sady nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="7c76b-118">Create endpoint and load balancer set</span></span>

<span data-ttu-id="7c76b-119">Hello scénář předpokládá hello virtuálních počítačů "web1" a "web2" byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="7c76b-119">hello scenario assumes hello virtual machines "web1" and "web2" were created.</span></span>
<span data-ttu-id="7c76b-120">Tento průvodce vytvoří sadu nástroje pro vyrovnávání zatížení používající port 80 jako veřejný port a port 80 jako místní port.</span><span class="sxs-lookup"><span data-stu-id="7c76b-120">This guide will create a load balancer set using port 80 as public port and port 80 as local port.</span></span> <span data-ttu-id="7c76b-121">Port testu je také konfigurován na portu 80 a nástroj pro vyrovnávání zatížení s názvem hello nastavte "lbset".</span><span class="sxs-lookup"><span data-stu-id="7c76b-121">A probe port is also configured on port 80 and named hello load balancer set "lbset".</span></span>

### <a name="step-1"></a><span data-ttu-id="7c76b-122">Krok 1</span><span class="sxs-lookup"><span data-stu-id="7c76b-122">Step 1</span></span>

<span data-ttu-id="7c76b-123">Vytvoření první koncový bod hello a nastavit pomocí nástroje pro vyrovnání zatížení `azure network vm endpoint create` pro virtuální počítač "web1".</span><span class="sxs-lookup"><span data-stu-id="7c76b-123">Create hello first endpoint and load balancer set using `azure network vm endpoint create` for virtual machine "web1".</span></span>

```azurecli
azure vm endpoint create web1 80 --local-port 80 --protocol tcp --probe-port 80 --load-balanced-set-name lbset
```

## <a name="step-2"></a><span data-ttu-id="7c76b-124">Krok 2</span><span class="sxs-lookup"><span data-stu-id="7c76b-124">Step 2</span></span>

<span data-ttu-id="7c76b-125">Přidáte sadu Nástroje pro vyrovnávání zatížení toohello druhý virtuální počítač "webu 2".</span><span class="sxs-lookup"><span data-stu-id="7c76b-125">Add a second virtual machine "web2" toohello load balancer set.</span></span>

```azurecli
azure vm endpoint create web2 80 --local-port 80 --protocol tcp --probe-port 80 --load-balanced-set-name lbset
```

## <a name="step-3"></a><span data-ttu-id="7c76b-126">Krok 3</span><span class="sxs-lookup"><span data-stu-id="7c76b-126">Step 3</span></span>

<span data-ttu-id="7c76b-127">Ověření konfigurace služby Vyrovnávání zatížení hello pomocí `azure vm show` .</span><span class="sxs-lookup"><span data-stu-id="7c76b-127">Verify hello load balancer configuration using `azure vm show` .</span></span>

```azurecli
azure vm show web1
```

<span data-ttu-id="7c76b-128">výstup Hello bude:</span><span class="sxs-lookup"><span data-stu-id="7c76b-128">hello output will be:</span></span>

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

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a><span data-ttu-id="7c76b-129">Vytvoření koncového bodu vzdálené plochy pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="7c76b-129">Create a remote desktop endpoint for a virtual machine</span></span>

<span data-ttu-id="7c76b-130">Můžete vytvořit vzdálené ploše koncový bod tooforward síťový provoz z místního portu veřejný port tooa pro konkrétní virtuální počítač pomocí `azure vm endpoint create`.</span><span class="sxs-lookup"><span data-stu-id="7c76b-130">You can create a remote desktop endpoint tooforward network traffic from a public port tooa local port for a specific virtual machine using `azure vm endpoint create`.</span></span>

```azurecli
azure vm endpoint create web1 54580 -k 3389
```

## <a name="remove-virtual-machine-from-load-balancer"></a><span data-ttu-id="7c76b-131">Odebrání virtuálního počítače z nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="7c76b-131">Remove virtual machine from load balancer</span></span>

<span data-ttu-id="7c76b-132">Máte toodelete hello koncový bod přidružené toohello sady Nástroje pro vyrovnávání zatížení z virtuálního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="7c76b-132">You have toodelete hello endpoint associated toohello load balancer set from hello virtual machine.</span></span> <span data-ttu-id="7c76b-133">Odebraný hello koncový bod virtuálního počítače hello nepatří toohello s vyrovnáváním zatížení už.</span><span class="sxs-lookup"><span data-stu-id="7c76b-133">Once hello endpoint is removed, hello virtual machine doesn't belong toohello load balancer set anymore.</span></span>

<span data-ttu-id="7c76b-134">Pomocí výše uvedeném příkladu hello, můžete odebrat hello koncového bodu pro virtuální počítač "web1" vytvořit z nástroje pro vyrovnávání zatížení "lbset" pomocí příkazu hello `azure vm endpoint delete`.</span><span class="sxs-lookup"><span data-stu-id="7c76b-134">Using hello example above, you can remove hello endpoint created for virtual machine "web1" from load balancer "lbset" using hello command `azure vm endpoint delete`.</span></span>

```azurecli
azure vm endpoint delete web1 tcp-80-80
```

> [!NOTE]
> <span data-ttu-id="7c76b-135">Můžete si prostudovat další koncové body toomanage možnosti pomocí příkazu hello`azure vm endpoint --help`</span><span class="sxs-lookup"><span data-stu-id="7c76b-135">You can explore more options toomanage endpoints using hello command `azure vm endpoint --help`</span></span>

## <a name="next-steps"></a><span data-ttu-id="7c76b-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7c76b-136">Next steps</span></span>

[<span data-ttu-id="7c76b-137">Začínáme s konfigurací interního nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="7c76b-137">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="7c76b-138">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="7c76b-138">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="7c76b-139">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="7c76b-139">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
