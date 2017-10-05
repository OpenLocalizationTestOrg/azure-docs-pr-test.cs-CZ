---
title: "Řídit směrování a virtuální zařízení ve službě Azure - prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak řídit směrování a virtuální zařízení pomocí prostředí PowerShell."
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
ms.openlocfilehash: 3ab24f193c74449ae7414b4ea0675c0aae0211f4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-user-defined-routes-udr-using-powershell"></a><span data-ttu-id="a36c7-103">Vytvoření trasy definované uživatelem (UDR) pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="a36c7-103">Create User-Defined Routes (UDR) using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="a36c7-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a36c7-104">PowerShell</span></span>](virtual-network-create-udr-arm-ps.md)
> * [<span data-ttu-id="a36c7-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a36c7-105">Azure CLI</span></span>](virtual-network-create-udr-arm-cli.md)
> * [<span data-ttu-id="a36c7-106">Šablona</span><span class="sxs-lookup"><span data-stu-id="a36c7-106">Template</span></span>](virtual-network-create-udr-arm-template.md)
> * [<span data-ttu-id="a36c7-107">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="a36c7-107">PowerShell (Classic)</span></span>](virtual-network-create-udr-classic-ps.md)
> * [<span data-ttu-id="a36c7-108">Rozhraní příkazového řádku (Classic)</span><span class="sxs-lookup"><span data-stu-id="a36c7-108">CLI (Classic)</span></span>](virtual-network-create-udr-classic-cli.md)


[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="a36c7-109">Než začnete pracovat s prostředky Azure, je potřeba si uvědomit, že Azure má v současné době dva modely nasazení: Azure Resource Manager a klasický.</span><span class="sxs-lookup"><span data-stu-id="a36c7-109">Before you work with Azure resources, it's important to understand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="a36c7-110">Před zahájením práce s jakýmikoli prostředky Azure se ujistěte, že rozumíte [modelům nasazení a příslušným nástrojům](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="a36c7-110">Make sure you understand [deployment models and tools](../azure-resource-manager/resource-manager-deployment-model.md) before you work with any Azure resource.</span></span> <span data-ttu-id="a36c7-111">Dokumentaci k různým nástrojům můžete zobrazit kliknutím na karty v horní části tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="a36c7-111">You can view the documentation for different tools by clicking the tabs at the top of this article.</span></span>
>

<span data-ttu-id="a36c7-112">Tento článek se týká modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a36c7-112">This article covers the Resource Manager deployment model.</span></span> <span data-ttu-id="a36c7-113">Můžete také [vytvořit udr v modelu nasazení classic](virtual-network-create-udr-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="a36c7-113">You can also [create UDRs in the classic deployment model](virtual-network-create-udr-classic-ps.md).</span></span>

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

<span data-ttu-id="a36c7-114">Ukázka jednoduché prostředí už vytvořený očekávat níže uvedené příkazy prostředí PowerShell založené na výše uvedené scénáře.</span><span class="sxs-lookup"><span data-stu-id="a36c7-114">The sample PowerShell commands below expect a simple environment already created based on the scenario above.</span></span> <span data-ttu-id="a36c7-115">Pokud chcete ke spuštění příkazů, jak jsou zobrazeny v tomto dokumentu, nasazením nejprve vytvořit testovací prostředí [této šablony](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), klikněte na tlačítko **nasadit do Azure**, nahradí výchozí hodnoty parametrů v případě potřeby a postupujte podle pokynů v portálu.</span><span class="sxs-lookup"><span data-stu-id="a36c7-115">If you want to run the commands as they are displayed in this document, first build the test environment by deploying [this template](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), click **Deploy to Azure**, replace the default parameter values if necessary, and follow the instructions in the portal.</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a><span data-ttu-id="a36c7-116">Vytvoření UDR front-end podsítě</span><span class="sxs-lookup"><span data-stu-id="a36c7-116">Create the UDR for the front-end subnet</span></span>
<span data-ttu-id="a36c7-117">Pokud chcete vytvořit směrovací tabulku a směrování, které jsou potřebné pro front-endu podsíť závislosti na scénáři výše, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a36c7-117">To create the route table and route needed for the front-end subnet based on the scenario above, complete the following steps:</span></span>

1. <span data-ttu-id="a36c7-118">Vytvoření používá k odesílání veškerý provoz, jehož k podsíti back-end (192.168.2.0/24) k přesměrování na trasu **FW1** virtuální zařízení (192.168.0.4).</span><span class="sxs-lookup"><span data-stu-id="a36c7-118">Create a route used to send all traffic destined to the back-end subnet (192.168.2.0/24) to be routed to the **FW1** virtual appliance (192.168.0.4).</span></span>

    ```powershell
    $route = New-AzureRmRouteConfig -Name RouteToBackEnd `
    -AddressPrefix 192.168.2.0/24 -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

2. <span data-ttu-id="a36c7-119">Vytvořit směrovací tabulku s názvem **UDR front-endu** v **westus** oblast, která obsahuje trasy.</span><span class="sxs-lookup"><span data-stu-id="a36c7-119">Create a route table named **UDR-FrontEnd** in the **westus** region that contains the route.</span></span>

    ```powershell
    $routeTable = New-AzureRmRouteTable -ResourceGroupName TestRG -Location westus `
    -Name UDR-FrontEnd -Route $route
    ```

3. <span data-ttu-id="a36c7-120">Vytvoření proměnné, která obsahuje virtuální síť, kde je podsíť.</span><span class="sxs-lookup"><span data-stu-id="a36c7-120">Create a variable that contains the VNet where the subnet is.</span></span> <span data-ttu-id="a36c7-121">V našem scénáři je název sítě VNet **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="a36c7-121">In our scenario, the VNet is named **TestVNet**.</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

4. <span data-ttu-id="a36c7-122">Přidružení směrovací tabulka vytvořili výše **front-endu** podsítě.</span><span class="sxs-lookup"><span data-stu-id="a36c7-122">Associate the route table created above to the **FrontEnd** subnet.</span></span>

    ```powershell
    Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd `
    -AddressPrefix 192.168.1.0/24 -RouteTable $routeTable
    ```

    > [!WARNING]
    > <span data-ttu-id="a36c7-123">Výstup výše uvedeného příkazu zobrazuje obsah pro objekt konfigurace virtuální sítě, který existuje pouze na počítače, kde běží prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a36c7-123">The output for the command above shows the content for the virtual network configuration object, which only exists on the computer where you are running PowerShell.</span></span> <span data-ttu-id="a36c7-124">Je třeba spustit **Set-AzureVirtualNetwork** rutiny uložit tato nastavení do Azure.</span><span class="sxs-lookup"><span data-stu-id="a36c7-124">You need to run the **Set-AzureVirtualNetwork** cmdlet to save these settings to Azure.</span></span>
    > 

5. <span data-ttu-id="a36c7-125">Uložte novou konfiguraci podsítě v Azure.</span><span class="sxs-lookup"><span data-stu-id="a36c7-125">Save the new subnet configuration in Azure.</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    <span data-ttu-id="a36c7-126">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="a36c7-126">Expected output:</span></span>
   
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

## <a name="create-the-udr-for-the-back-end-subnet"></a><span data-ttu-id="a36c7-127">Vytvoření UDR pro podsíť back-end</span><span class="sxs-lookup"><span data-stu-id="a36c7-127">Create the UDR for the back-end subnet</span></span>

<span data-ttu-id="a36c7-128">Pokud chcete vytvořit směrovací tabulku a směrování, které jsou potřeba pro back-end podsíť závislosti na scénáři výše, postupujte podle následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="a36c7-128">To create the route table and route needed for the back-end subnet based on the scenario above, follow the steps below.</span></span>

1. <span data-ttu-id="a36c7-129">Vytvořit trasu používá k odesílání veškerý provoz, jehož front-end podsítě (192.168.1.0/24), která se směrovat na **FW1** virtuální zařízení (192.168.0.4).</span><span class="sxs-lookup"><span data-stu-id="a36c7-129">Create a route used to send all traffic destined to the front-end subnet (192.168.1.0/24) to be routed to the **FW1** virtual appliance (192.168.0.4).</span></span>

    ```powershell
    $route = New-AzureRmRouteConfig -Name RouteToFrontEnd `
    -AddressPrefix 192.168.1.0/24 -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

2. <span data-ttu-id="a36c7-130">Vytvořit směrovací tabulku s názvem **UDR back-end** v **uswest** oblast, která obsahuje trasy vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="a36c7-130">Create a route table named **UDR-BackEnd** in the **uswest** region that contains the route created above.</span></span>

    ```
    $routeTable = New-AzureRmRouteTable -ResourceGroupName TestRG -Location westus `
    -Name UDR-BackEnd -Route $route
    ```

3. <span data-ttu-id="a36c7-131">Přidružení směrovací tabulka vytvořili výše **back-end** podsítě.</span><span class="sxs-lookup"><span data-stu-id="a36c7-131">Associate the route table created above to the **BackEnd** subnet.</span></span>

    ```powershell
    Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name BackEnd `
    -AddressPrefix 192.168.2.0/24 -RouteTable $routeTable
    ```

4. <span data-ttu-id="a36c7-132">Uložte novou konfiguraci podsítě v Azure.</span><span class="sxs-lookup"><span data-stu-id="a36c7-132">Save the new subnet configuration in Azure.</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    <span data-ttu-id="a36c7-133">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="a36c7-133">Expected output:</span></span>
   
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

## <a name="enable-ip-forwarding-on-fw1"></a><span data-ttu-id="a36c7-134">Povolení předávání IP na FW1</span><span class="sxs-lookup"><span data-stu-id="a36c7-134">Enable IP forwarding on FW1</span></span>
<span data-ttu-id="a36c7-135">Povolení předávání IP v síťový adaptér používá **FW1**, použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="a36c7-135">To enable IP forwarding in the NIC used by **FW1**, follow the steps below.</span></span>

1. <span data-ttu-id="a36c7-136">Vytvoření proměnné, která obsahuje nastavení pro síťový adaptér používá FW1.</span><span class="sxs-lookup"><span data-stu-id="a36c7-136">Create a variable that contains the settings for the NIC used by FW1.</span></span> <span data-ttu-id="a36c7-137">V našem scénáři je síťový adaptér s názvem **NICFW1**.</span><span class="sxs-lookup"><span data-stu-id="a36c7-137">In our scenario, the NIC is named **NICFW1**.</span></span>

    ```powershell
    $nicfw1 = Get-AzureRmNetworkInterface -ResourceGroupName TestRG -Name NICFW1
    ```

2. <span data-ttu-id="a36c7-138">Povolení předávání IP a uložte nastavení síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="a36c7-138">Enable IP forwarding, and save the NIC settings.</span></span>

    ```powershell
    $nicfw1.EnableIPForwarding = 1
    Set-AzureRmNetworkInterface -NetworkInterface $nicfw1
    ```
   
    <span data-ttu-id="a36c7-139">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="a36c7-139">Expected output:</span></span>
   
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

