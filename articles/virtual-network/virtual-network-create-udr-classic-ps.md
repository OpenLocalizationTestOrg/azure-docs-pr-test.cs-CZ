---
title: "Řídit směrování v Azure Virtual Network - PowerShell – Classic | Microsoft Docs"
description: "Zjistěte, jak řídit směrování v virtuální sítě pomocí prostředí PowerShell | Classic"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: d8d07c16-cbe5-4536-acd6-870269346fe3
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.openlocfilehash: e9564d223cb85529f1fa97bc398d35c6debcedae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="control-routing-and-use-virtual-appliances-classic-using-powershell"></a><span data-ttu-id="b740b-103">Řídit směrování a použití virtuálních zařízení (klasické) pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="b740b-103">Control routing and use virtual appliances (classic) using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="b740b-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b740b-104">PowerShell</span></span>](virtual-network-create-udr-arm-ps.md)
> * [<span data-ttu-id="b740b-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b740b-105">Azure CLI</span></span>](virtual-network-create-udr-arm-cli.md)
> * [<span data-ttu-id="b740b-106">Šablona</span><span class="sxs-lookup"><span data-stu-id="b740b-106">Template</span></span>](virtual-network-create-udr-arm-template.md)
> * [<span data-ttu-id="b740b-107">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="b740b-107">PowerShell (Classic)</span></span>](virtual-network-create-udr-classic-ps.md)
> * [<span data-ttu-id="b740b-108">Rozhraní příkazového řádku (Classic)</span><span class="sxs-lookup"><span data-stu-id="b740b-108">CLI (Classic)</span></span>](virtual-network-create-udr-classic-cli.md)

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="b740b-109">Než začnete pracovat s prostředky Azure, je potřeba si uvědomit, že Azure má v současné době dva modely nasazení: Azure Resource Manager a klasický.</span><span class="sxs-lookup"><span data-stu-id="b740b-109">Before you work with Azure resources, it's important to understand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="b740b-110">Před zahájením práce s jakýmikoli prostředky Azure se ujistěte, že rozumíte [modelům nasazení a příslušným nástrojům](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="b740b-110">Make sure you understand [deployment models and tools](../azure-resource-manager/resource-manager-deployment-model.md) before you work with any Azure resource.</span></span> <span data-ttu-id="b740b-111">Dokumentaci k různým nástrojům můžete zobrazit výběrem možnosti v horní části tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="b740b-111">You can view the documentation for different tools by selecting an option at the top of this article.</span></span> <span data-ttu-id="b740b-112">Tento článek se týká modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="b740b-112">This article covers the classic deployment model.</span></span>
> 

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

<span data-ttu-id="b740b-113">Ukázka prostředí Azure PowerShell níže uvedené příkazy očekávat jednoduché prostředí už vytvořený založené na výše uvedené scénáře.</span><span class="sxs-lookup"><span data-stu-id="b740b-113">The sample Azure PowerShell commands below expect a simple environment already created based on the scenario above.</span></span> <span data-ttu-id="b740b-114">Pokud chcete ke spuštění příkazů, jak jsou zobrazeny v tomto dokumentu, vytvoření prostředí ukazuje [vytvoření virtuální sítě (klasické) pomocí prostředí PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md).</span><span class="sxs-lookup"><span data-stu-id="b740b-114">If you want to run the commands as they are displayed in this document, create the environment shown in [create a VNet (classic) using PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md).</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a><span data-ttu-id="b740b-115">Vytvoření UDR pro podsítě front end</span><span class="sxs-lookup"><span data-stu-id="b740b-115">Create the UDR for the front end subnet</span></span>
<span data-ttu-id="b740b-116">Pokud chcete vytvořit směrovací tabulku a směrování, které jsou potřebné pro podsítě front end závislosti na scénáři výše, postupujte podle následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="b740b-116">To create the route table and route needed for the front end subnet based on the scenario above, follow the steps below.</span></span>

1. <span data-ttu-id="b740b-117">Spusťte následující příkaz, který vytvořit směrovací tabulku front-end podsítě:</span><span class="sxs-lookup"><span data-stu-id="b740b-117">Run the following command to create a route table for the front-end subnet:</span></span>

    ```powershell
    New-AzureRouteTable -Name UDR-FrontEnd -Location uswest `
    -Label "Route table for front end subnet"
    ```

2. <span data-ttu-id="b740b-118">Spusťte následující příkaz k vytvoření trasy ve směrovací tabulce odeslat veškerý provoz, jehož k podsíti back-end (192.168.2.0/24) na **FW1** virtuálních počítačů (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="b740b-118">Run the following command to create a route in the route table to send all traffic destined to the back-end subnet (192.168.2.0/24) to the **FW1** VM (192.168.0.4):</span></span>

    ```powershell
    Get-AzureRouteTable UDR-FrontEnd `
    |Set-AzureRoute -RouteName RouteToBackEnd -AddressPrefix 192.168.2.0/24 `
    -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

3. <span data-ttu-id="b740b-119">Spusťte následující příkaz k přiřazení směrovací tabulka s **front-endu** podsítě:</span><span class="sxs-lookup"><span data-stu-id="b740b-119">Run the following command to associate the route table with the **FrontEnd** subnet:</span></span>

    ```powershell
    Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
    -SubnetName FrontEnd `
    -RouteTableName UDR-FrontEnd
    ```

## <a name="create-the-udr-for-the-back-end-subnet"></a><span data-ttu-id="b740b-120">Vytvoření UDR pro podsíť back-end</span><span class="sxs-lookup"><span data-stu-id="b740b-120">Create the UDR for the back-end subnet</span></span>
<span data-ttu-id="b740b-121">Pokud chcete vytvořit směrovací tabulku a směrování, které jsou potřeba pro back-end podsíť závislosti na scénáři, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b740b-121">To create the route table and route needed for the back end subnet based on the scenario, complete the following steps:</span></span>

1. <span data-ttu-id="b740b-122">Spusťte následující příkaz a vytvořte tabulku směrování pro podsíť back-end:</span><span class="sxs-lookup"><span data-stu-id="b740b-122">Run the following command to create a route table for the back-end subnet:</span></span>

    ```powershell
    New-AzureRouteTable -Name UDR-BackEnd `
    -Location uswest `
    -Label "Route table for back end subnet"
    ```

2. <span data-ttu-id="b740b-123">Spusťte následující příkaz k vytvoření trasy ve směrovací tabulce odeslat veškerý provoz, jehož klientské podsíti (192.168.1.0/24) na **FW1** virtuálních počítačů (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="b740b-123">Run the following command to create a route in the route table to send all traffic destined to the front-end subnet (192.168.1.0/24) to the **FW1** VM (192.168.0.4):</span></span>

    ```powershell
    Get-AzureRouteTable UDR-BackEnd
    | Set-AzureRoute `
    -RouteName RouteToFrontEnd `
    -AddressPrefix 192.168.1.0/24 `
    -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

3. <span data-ttu-id="b740b-124">Spusťte následující příkaz k přiřazení směrovací tabulka s **back-end** podsítě:</span><span class="sxs-lookup"><span data-stu-id="b740b-124">Run the following command to associate the route table with the **BackEnd** subnet:</span></span>

    ```powershell
    Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
    -SubnetName BackEnd `
    -RouteTableName UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-the-fw1-vm"></a><span data-ttu-id="b740b-125">Povolení předávání IP ve virtuálním počítači FW1</span><span class="sxs-lookup"><span data-stu-id="b740b-125">Enable IP forwarding on the FW1 VM</span></span>

<span data-ttu-id="b740b-126">Pokud chcete povolit předávání ve virtuálním počítači FW1 protokolu IP, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b740b-126">To enable IP forwarding in the FW1 VM, complete the following steps:</span></span>

1. <span data-ttu-id="b740b-127">Spusťte následující příkaz a zkontrolujte stav předávání IP:</span><span class="sxs-lookup"><span data-stu-id="b740b-127">Run the following command to check the status of IP forwarding:</span></span>

    ```powershell
    Get-AzureVM -Name FW1 -ServiceName TestRGFW `
    | Get-AzureIPForwarding
    ```

2. <span data-ttu-id="b740b-128">Spusťte následující příkaz k povolení předávání protokolu IP pro *FW1* virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="b740b-128">Run the following command to enable IP forwarding for the *FW1* VM:</span></span>

    ```powershell
    Get-AzureVM -Name FW1 -ServiceName TestRGFW `
    | Set-AzureIPForwarding -Enable
    ```
