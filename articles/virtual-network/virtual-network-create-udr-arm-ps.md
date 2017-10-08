---
title: "aaaControl směrování a virtuální zařízení ve službě Azure - prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak toocontrol směrování a virtuální zařízení pomocí prostředí PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 9582fdaa-249c-4c98-9618-8c30d496940f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2016
ms.author: jdial
ms.openlocfilehash: b7b8717529eb2cd8b1d28b8ab9c6e21159d14882
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-user-defined-routes-udr-using-powershell"></a><span data-ttu-id="add00-103">Vytvoření trasy definované uživatelem (UDR) pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="add00-103">Create User-Defined Routes (UDR) using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="add00-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="add00-104">PowerShell</span></span>](virtual-network-create-udr-arm-ps.md)
> * [<span data-ttu-id="add00-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="add00-105">Azure CLI</span></span>](virtual-network-create-udr-arm-cli.md)
> * [<span data-ttu-id="add00-106">Šablona</span><span class="sxs-lookup"><span data-stu-id="add00-106">Template</span></span>](virtual-network-create-udr-arm-template.md)
> * [<span data-ttu-id="add00-107">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="add00-107">PowerShell (Classic)</span></span>](virtual-network-create-udr-classic-ps.md)
> * [<span data-ttu-id="add00-108">Rozhraní příkazového řádku (Classic)</span><span class="sxs-lookup"><span data-stu-id="add00-108">CLI (Classic)</span></span>](virtual-network-create-udr-classic-cli.md)


[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="add00-109">Než začnete pracovat s prostředky Azure, je důležité toounderstand Azure aktuálně má dva modely nasazení: Azure Resource Manager a Klasický model.</span><span class="sxs-lookup"><span data-stu-id="add00-109">Before you work with Azure resources, it's important toounderstand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="add00-110">Před zahájením práce s jakýmikoli prostředky Azure se ujistěte, že rozumíte [modelům nasazení a příslušným nástrojům](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="add00-110">Make sure you understand [deployment models and tools](../azure-resource-manager/resource-manager-deployment-model.md) before you work with any Azure resource.</span></span> <span data-ttu-id="add00-111">Hello dokumentaci k různým nástrojům můžete zobrazit kliknutím na karty hello hello horní části tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="add00-111">You can view hello documentation for different tools by clicking hello tabs at hello top of this article.</span></span>
>

<span data-ttu-id="add00-112">Tento článek se týká modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="add00-112">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="add00-113">Můžete také [vytvořit udr v modelu nasazení classic hello](virtual-network-create-udr-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="add00-113">You can also [create UDRs in hello classic deployment model](virtual-network-create-udr-classic-ps.md).</span></span>

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

<span data-ttu-id="add00-114">Ukázka Hello jednoduché prostředí už vytvořený očekávat níže uvedené příkazy prostředí PowerShell založené na scénář hello výše.</span><span class="sxs-lookup"><span data-stu-id="add00-114">hello sample PowerShell commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="add00-115">Pokud chcete příkazy hello toorun, jak jsou zobrazeny v tomto dokumentu, vytvoření nasazením nejprve hello testovací prostředí [této šablony](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), klikněte na tlačítko **nasazení tooAzure**, nahraďte hello výchozí hodnoty parametrů Pokud potřeby a postupujte podle pokynů hello v hello portálu.</span><span class="sxs-lookup"><span data-stu-id="add00-115">If you want toorun hello commands as they are displayed in this document, first build hello test environment by deploying [this template](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-hello-udr-for-hello-front-end-subnet"></a><span data-ttu-id="add00-116">Vytvoření hello UDR podsítě front-endu hello</span><span class="sxs-lookup"><span data-stu-id="add00-116">Create hello UDR for hello front-end subnet</span></span>
<span data-ttu-id="add00-117">toocreate hello směrovací tabulku a směrování, které jsou potřebné pro front-endu podsíť hello podle hello scénář výše, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="add00-117">toocreate hello route table and route needed for hello front-end subnet based on hello scenario above, complete hello following steps:</span></span>

1. <span data-ttu-id="add00-118">Vytvořit trasu použít toosend toohello podsítě back-end (192.168.2.0/24) toobe směrovat všechny přenosy určené toohello **FW1** virtuální zařízení (192.168.0.4).</span><span class="sxs-lookup"><span data-stu-id="add00-118">Create a route used toosend all traffic destined toohello back-end subnet (192.168.2.0/24) toobe routed toohello **FW1** virtual appliance (192.168.0.4).</span></span>

    ```powershell
    $route = New-AzureRmRouteConfig -Name RouteToBackEnd `
    -AddressPrefix 192.168.2.0/24 -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

2. <span data-ttu-id="add00-119">Vytvořit směrovací tabulku s názvem **UDR front-endu** v hello **westus** oblast, která obsahuje trasy hello.</span><span class="sxs-lookup"><span data-stu-id="add00-119">Create a route table named **UDR-FrontEnd** in hello **westus** region that contains hello route.</span></span>

    ```powershell
    $routeTable = New-AzureRmRouteTable -ResourceGroupName TestRG -Location westus `
    -Name UDR-FrontEnd -Route $route
    ```

3. <span data-ttu-id="add00-120">Vytvoření proměnné, která obsahuje hello virtuální síť, kde je hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="add00-120">Create a variable that contains hello VNet where hello subnet is.</span></span> <span data-ttu-id="add00-121">V našem scénáři hello virtuální sítě je název **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="add00-121">In our scenario, hello VNet is named **TestVNet**.</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

4. <span data-ttu-id="add00-122">Směrovací tabulka přidružení hello vytvořili výše toohello **front-endu** podsítě.</span><span class="sxs-lookup"><span data-stu-id="add00-122">Associate hello route table created above toohello **FrontEnd** subnet.</span></span>

    ```powershell
    Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd `
    -AddressPrefix 192.168.1.0/24 -RouteTable $routeTable
    ```

    > [!WARNING]
    > <span data-ttu-id="add00-123">výstup Hello výše hello příkazu zobrazuje obsah hello hello virtuální sítě konfigurace objektu, který existuje pouze na hello počítače, kde běží prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="add00-123">hello output for hello command above shows hello content for hello virtual network configuration object, which only exists on hello computer where you are running PowerShell.</span></span> <span data-ttu-id="add00-124">Je třeba toorun hello **Set-AzureVirtualNetwork** toosave rutiny tooAzure těchto nastavení.</span><span class="sxs-lookup"><span data-stu-id="add00-124">You need toorun hello **Set-AzureVirtualNetwork** cmdlet toosave these settings tooAzure.</span></span>
    > 

5. <span data-ttu-id="add00-125">Uložte novou konfiguraci podsítě hello v Azure.</span><span class="sxs-lookup"><span data-stu-id="add00-125">Save hello new subnet configuration in Azure.</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    <span data-ttu-id="add00-126">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="add00-126">Expected output:</span></span>
   
        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : westus
        Id                : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"[Id]"
        ProvisioningState : Succeeded
        Tags              : 
                            Name         Value
                            ===========  =====
                            displayName  VNet 
   
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": null
                            }
        NetworkInterfaces : null
        Subnets           : [
                                ...,
                              {
                                "Name": "FrontEnd",
                                "Etag": "W/\"[Id]\"",
                                "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                "AddressPrefix": "192.168.1.0/24",
                                "IpConfigurations": [
                                  {
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConfigurations/ipconfig1"
                                  },
                                  {
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                                  }
                                ],
                                "NetworkSecurityGroup": {
                                  "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                },
                                "RouteTable": {
                                  "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                                },
                                "ProvisioningState": "Succeeded"
                              },
                                ...
                            ]    

## <a name="create-hello-udr-for-hello-back-end-subnet"></a><span data-ttu-id="add00-127">Vytvoření hello UDR pro podsíť back-end hello</span><span class="sxs-lookup"><span data-stu-id="add00-127">Create hello UDR for hello back-end subnet</span></span>

<span data-ttu-id="add00-128">toocreate hello směrovací tabulku a směrování pro podsíť back-end hello hello scénář výše, podle potřeby postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="add00-128">toocreate hello route table and route needed for hello back-end subnet based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="add00-129">Vytvořit trasu použít toosend toohello front-end podsíť (192.168.1.0/24) toobe směrovat všechny přenosy určené toohello **FW1** virtuální zařízení (192.168.0.4).</span><span class="sxs-lookup"><span data-stu-id="add00-129">Create a route used toosend all traffic destined toohello front-end subnet (192.168.1.0/24) toobe routed toohello **FW1** virtual appliance (192.168.0.4).</span></span>

    ```powershell
    $route = New-AzureRmRouteConfig -Name RouteToFrontEnd `
    -AddressPrefix 192.168.1.0/24 -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

2. <span data-ttu-id="add00-130">Vytvořit směrovací tabulku s názvem **UDR back-end** v hello **uswest** oblast, která obsahuje trasy hello vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="add00-130">Create a route table named **UDR-BackEnd** in hello **uswest** region that contains hello route created above.</span></span>

    ```
    $routeTable = New-AzureRmRouteTable -ResourceGroupName TestRG -Location westus `
    -Name UDR-BackEnd -Route $route
    ```

3. <span data-ttu-id="add00-131">Směrovací tabulka přidružení hello vytvořili výše toohello **back-end** podsítě.</span><span class="sxs-lookup"><span data-stu-id="add00-131">Associate hello route table created above toohello **BackEnd** subnet.</span></span>

    ```powershell
    Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name BackEnd `
    -AddressPrefix 192.168.2.0/24 -RouteTable $routeTable
    ```

4. <span data-ttu-id="add00-132">Uložte novou konfiguraci podsítě hello v Azure.</span><span class="sxs-lookup"><span data-stu-id="add00-132">Save hello new subnet configuration in Azure.</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    <span data-ttu-id="add00-133">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="add00-133">Expected output:</span></span>
   
        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : westus
        Id                : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"[Id]"
        ProvisioningState : Succeeded
        Tags              : 
                            Name         Value
                            ===========  =====
                            displayName  VNet 
   
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": null
                            }
        NetworkInterfaces : null
        Subnets           : [
                              ...,
                              {
                                "Name": "BackEnd",
                                "Etag": "W/\"[Id]\"",
                                "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                "AddressPrefix": "192.168.2.0/24",
                                "IpConfigurations": [
                                  {
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICSQL2/ipConfigurations/ipconfig1"
                                  },
                                  {
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICSQL1/ipConfigurations/ipconfig1"
                                  }
                                ],
                                "NetworkSecurityGroup": {
                                  "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BacEnd"
                                },
                                "RouteTable": {
                                  "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd"
                                },
                                "ProvisioningState": "Succeeded"
                              }
                            ]

## <a name="enable-ip-forwarding-on-fw1"></a><span data-ttu-id="add00-134">Povolení předávání IP na FW1</span><span class="sxs-lookup"><span data-stu-id="add00-134">Enable IP forwarding on FW1</span></span>
<span data-ttu-id="add00-135">předávání IP tooenable v hello síťový adaptér používá **FW1**, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="add00-135">tooenable IP forwarding in hello NIC used by **FW1**, follow hello steps below.</span></span>

1. <span data-ttu-id="add00-136">Vytvoření proměnné, která obsahuje hello nastavení pro hello používá FW1 síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="add00-136">Create a variable that contains hello settings for hello NIC used by FW1.</span></span> <span data-ttu-id="add00-137">V našem scénáři hello síťový adaptér je název **NICFW1**.</span><span class="sxs-lookup"><span data-stu-id="add00-137">In our scenario, hello NIC is named **NICFW1**.</span></span>

    ```powershell
    $nicfw1 = Get-AzureRmNetworkInterface -ResourceGroupName TestRG -Name NICFW1
    ```

2. <span data-ttu-id="add00-138">Povolení předávání IP a uložte nastavení hello síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="add00-138">Enable IP forwarding, and save hello NIC settings.</span></span>

    ```powershell
    $nicfw1.EnableIPForwarding = 1
    Set-AzureRmNetworkInterface -NetworkInterface $nicfw1
    ```
   
    <span data-ttu-id="add00-139">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="add00-139">Expected output:</span></span>
   
        Name                 : NICFW1
        ResourceGroupName    : TestRG
        Location             : westus
        Id                   : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICFW1
        Etag                 : W/"[Id]"
        ProvisioningState    : Succeeded
        Tags                 : 
                               Name         Value                  
                               ===========  =======================
                               displayName  NetworkInterfaces - DMZ
   
        VirtualMachine       : {
                                 "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/FW1"
                               }
        IpConfigurations     : [
                                 {
                                   "Name": "ipconfig1",
                                   "Etag": "W/\"[Id]\"",
                                   "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICFW1/ipConfigurations/ipconfig1",
                                   "PrivateIpAddress": "192.168.0.4",
                                   "PrivateIpAllocationMethod": "Static",
                                   "Subnet": {
                                     "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/DMZ"
                                   },
                                   "PublicIpAddress": {
                                     "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/PIPFW1"
                                   },
                                   "LoadBalancerBackendAddressPools": [],
                                   "LoadBalancerInboundNatRules": [],
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]
        DnsSettings          : {
                                 "DnsServers": [],
                                 "AppliedDnsServers": [],
                                 "InternalDnsNameLabel": null,
                                 "InternalFqdn": null
                               }
        EnableIPForwarding   : True
        NetworkSecurityGroup : null
        Primary              : True

