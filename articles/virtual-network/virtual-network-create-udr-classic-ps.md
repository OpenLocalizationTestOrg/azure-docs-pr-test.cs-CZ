---
title: "aaaControl směrování v Azure Virtual Network - PowerShell – Classic | Microsoft Docs"
description: "Zjistěte, jak toocontrol směrování v virtuální sítě pomocí prostředí PowerShell | Classic"
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
ms.openlocfilehash: 36edf263fb434d5fb13310d4324da20e57f016a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="control-routing-and-use-virtual-appliances-classic-using-powershell"></a><span data-ttu-id="e9977-103">Řídit směrování a použití virtuálních zařízení (klasické) pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="e9977-103">Control routing and use virtual appliances (classic) using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="e9977-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e9977-104">PowerShell</span></span>](virtual-network-create-udr-arm-ps.md)
> * [<span data-ttu-id="e9977-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e9977-105">Azure CLI</span></span>](virtual-network-create-udr-arm-cli.md)
> * [<span data-ttu-id="e9977-106">Šablona</span><span class="sxs-lookup"><span data-stu-id="e9977-106">Template</span></span>](virtual-network-create-udr-arm-template.md)
> * [<span data-ttu-id="e9977-107">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="e9977-107">PowerShell (Classic)</span></span>](virtual-network-create-udr-classic-ps.md)
> * [<span data-ttu-id="e9977-108">Rozhraní příkazového řádku (Classic)</span><span class="sxs-lookup"><span data-stu-id="e9977-108">CLI (Classic)</span></span>](virtual-network-create-udr-classic-cli.md)

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="e9977-109">Než začnete pracovat s prostředky Azure, je důležité toounderstand Azure aktuálně má dva modely nasazení: Azure Resource Manager a Klasický model.</span><span class="sxs-lookup"><span data-stu-id="e9977-109">Before you work with Azure resources, it's important toounderstand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="e9977-110">Před zahájením práce s jakýmikoli prostředky Azure se ujistěte, že rozumíte [modelům nasazení a příslušným nástrojům](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="e9977-110">Make sure you understand [deployment models and tools](../azure-resource-manager/resource-manager-deployment-model.md) before you work with any Azure resource.</span></span> <span data-ttu-id="e9977-111">Hello dokumentaci k různým nástrojům můžete zobrazit výběrem možnosti v horní části hello tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="e9977-111">You can view hello documentation for different tools by selecting an option at hello top of this article.</span></span> <span data-ttu-id="e9977-112">Tento článek se týká modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="e9977-112">This article covers hello classic deployment model.</span></span>
> 

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

<span data-ttu-id="e9977-113">Ukázka Hello prostředí Azure PowerShell níže uvedené příkazy očekávat jednoduché prostředí už vytvořený podle hello scénář výše.</span><span class="sxs-lookup"><span data-stu-id="e9977-113">hello sample Azure PowerShell commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="e9977-114">Pokud chcete příkazy hello toorun, jak jsou zobrazeny v tomto dokumentu, vytvoření prostředí hello ukazuje [vytvoření virtuální sítě (klasické) pomocí prostředí PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md).</span><span class="sxs-lookup"><span data-stu-id="e9977-114">If you want toorun hello commands as they are displayed in this document, create hello environment shown in [create a VNet (classic) using PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md).</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-hello-udr-for-hello-front-end-subnet"></a><span data-ttu-id="e9977-115">Vytvoření hello UDR pro podsítě front end hello</span><span class="sxs-lookup"><span data-stu-id="e9977-115">Create hello UDR for hello front end subnet</span></span>
<span data-ttu-id="e9977-116">toocreate hello směrovací tabulku a směrování, které jsou potřebné pro podsítě front end hello podle hello scénář výše, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="e9977-116">toocreate hello route table and route needed for hello front end subnet based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="e9977-117">Spusťte následující příkaz toocreate hello tabulka směrování pro podsíť pro front-end hello:</span><span class="sxs-lookup"><span data-stu-id="e9977-117">Run hello following command toocreate a route table for hello front-end subnet:</span></span>

    ```powershell
    New-AzureRouteTable -Name UDR-FrontEnd -Location uswest `
    -Label "Route table for front end subnet"
    ```

2. <span data-ttu-id="e9977-118">Spusťte následující příkaz toocreate trasy v toosend tabulky trasy hello hello všechny přenosy určené toohello podsítě back-end (192.168.2.0/24) toohello **FW1** virtuálních počítačů (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="e9977-118">Run hello following command toocreate a route in hello route table toosend all traffic destined toohello back-end subnet (192.168.2.0/24) toohello **FW1** VM (192.168.0.4):</span></span>

    ```powershell
    Get-AzureRouteTable UDR-FrontEnd `
    |Set-AzureRoute -RouteName RouteToBackEnd -AddressPrefix 192.168.2.0/24 `
    -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

3. <span data-ttu-id="e9977-119">Spuštění hello následující příkaz tooassociate hello směrovací tabulku s hello **front-endu** podsítě:</span><span class="sxs-lookup"><span data-stu-id="e9977-119">Run hello following command tooassociate hello route table with hello **FrontEnd** subnet:</span></span>

    ```powershell
    Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
    -SubnetName FrontEnd `
    -RouteTableName UDR-FrontEnd
    ```

## <a name="create-hello-udr-for-hello-back-end-subnet"></a><span data-ttu-id="e9977-120">Vytvoření hello UDR pro podsíť back-end hello</span><span class="sxs-lookup"><span data-stu-id="e9977-120">Create hello UDR for hello back-end subnet</span></span>
<span data-ttu-id="e9977-121">toocreate hello směrovací tabulku a směrování, které jsou potřeba pro hello back ukončit podsítě podle hello scénář, dokončete hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e9977-121">toocreate hello route table and route needed for hello back end subnet based on hello scenario, complete hello following steps:</span></span>

1. <span data-ttu-id="e9977-122">Spusťte následující příkaz toocreate hello tabulka směrování pro podsíť back-end hello:</span><span class="sxs-lookup"><span data-stu-id="e9977-122">Run hello following command toocreate a route table for hello back-end subnet:</span></span>

    ```powershell
    New-AzureRouteTable -Name UDR-BackEnd `
    -Location uswest `
    -Label "Route table for back end subnet"
    ```

2. <span data-ttu-id="e9977-123">Spusťte následující příkaz toocreate trasy v toosend tabulky trasy hello hello všechny přenosy určené toohello front-end podsíť (192.168.1.0/24) toohello **FW1** virtuálních počítačů (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="e9977-123">Run hello following command toocreate a route in hello route table toosend all traffic destined toohello front-end subnet (192.168.1.0/24) toohello **FW1** VM (192.168.0.4):</span></span>

    ```powershell
    Get-AzureRouteTable UDR-BackEnd
    | Set-AzureRoute `
    -RouteName RouteToFrontEnd `
    -AddressPrefix 192.168.1.0/24 `
    -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

3. <span data-ttu-id="e9977-124">Spuštění hello následující příkaz tooassociate hello směrovací tabulku s hello **back-end** podsítě:</span><span class="sxs-lookup"><span data-stu-id="e9977-124">Run hello following command tooassociate hello route table with hello **BackEnd** subnet:</span></span>

    ```powershell
    Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
    -SubnetName BackEnd `
    -RouteTableName UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-hello-fw1-vm"></a><span data-ttu-id="e9977-125">Povolení předávání IP na hello FW1 virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="e9977-125">Enable IP forwarding on hello FW1 VM</span></span>

<span data-ttu-id="e9977-126">předávání IP tooenable v hello FW1 virtuálních počítačů, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e9977-126">tooenable IP forwarding in hello FW1 VM, complete hello following steps:</span></span>

1. <span data-ttu-id="e9977-127">Spusťte následující příkaz toocheck hello stav předávání IP hello:</span><span class="sxs-lookup"><span data-stu-id="e9977-127">Run hello following command toocheck hello status of IP forwarding:</span></span>

    ```powershell
    Get-AzureVM -Name FW1 -ServiceName TestRGFW `
    | Get-AzureIPForwarding
    ```

2. <span data-ttu-id="e9977-128">Hello spusťte následující příkaz předávání IP tooenable hello *FW1* virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="e9977-128">Run hello following command tooenable IP forwarding for hello *FW1* VM:</span></span>

    ```powershell
    Get-AzureVM -Name FW1 -ServiceName TestRGFW `
    | Set-AzureIPForwarding -Enable
    ```
