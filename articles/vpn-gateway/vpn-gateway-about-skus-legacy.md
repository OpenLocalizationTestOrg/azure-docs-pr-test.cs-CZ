---
title: "SKU brány starší virtuální síť Azure | Microsoft Docs"
description: "Starý virtuální sítě SKU brány."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: 3b2126b1ecd1613950bbf311ae08fafd4af0d51f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a><span data-ttu-id="3c3cf-103">Práce s SKU (starší verze SKU) brány virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="3c3cf-103">Working with virtual network gateway SKUs (legacy SKUs)</span></span>

<span data-ttu-id="3c3cf-104">Tento článek obsahuje informace o starší verze (starý) brány virtuální sítě SKU.</span><span class="sxs-lookup"><span data-stu-id="3c3cf-104">This article contains information about the legacy (old) virtual network gateway SKUs.</span></span> <span data-ttu-id="3c3cf-105">Starší verze SKU i nadále fungovat v obou modelech nasazení pro brány sítě VPN, které již byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="3c3cf-105">The legacy SKUs still work in both deployment models for VPN gateways that have already been created.</span></span> <span data-ttu-id="3c3cf-106">Classic brány VPN dál používat starší verze SKU pro existující brány i pro nové brány.</span><span class="sxs-lookup"><span data-stu-id="3c3cf-106">Classic VPN gateways continue to use the legacy SKUs, both for existing gateways, and for new gateways.</span></span> <span data-ttu-id="3c3cf-107">Při vytváření nového správce prostředků VPN Gateway, použijte nové SKU brány.</span><span class="sxs-lookup"><span data-stu-id="3c3cf-107">When creating new Resource Manager VPN gateways, use the new gateway SKUs.</span></span> <span data-ttu-id="3c3cf-108">Informace o nové SKU najdete v tématu [o službě VPN Gateway](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="3c3cf-108">For information about the new SKUs, see [About VPN Gateway](vpn-gateway-about-vpngateways.md).</span></span>

## <span data-ttu-id="3c3cf-109"><a name="gwsku"></a>SKU brány</span><span class="sxs-lookup"><span data-stu-id="3c3cf-109"><a name="gwsku"></a>Gateway SKUs</span></span>

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

## <span data-ttu-id="3c3cf-110"><a name="agg"></a>Odhadovaná agregovaná propustnost podle SKU</span><span class="sxs-lookup"><span data-stu-id="3c3cf-110"><a name="agg"></a>Estimated aggregate throughput by SKU</span></span>

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <span data-ttu-id="3c3cf-111"><a name="config"></a>Podporované konfigurace podle typu SKU a síť VPN</span><span class="sxs-lookup"><span data-stu-id="3c3cf-111"><a name="config"></a>Supported configurations by SKU and VPN type</span></span>

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <span data-ttu-id="3c3cf-112"><a name="resize"></a>Změnit velikost brány (změnit a skladová položka brány)</span><span class="sxs-lookup"><span data-stu-id="3c3cf-112"><a name="resize"></a>Resize a gateway (change a gateway SKU)</span></span>

<span data-ttu-id="3c3cf-113">Můžete změnit velikost skladová položka brány v rámci stejné řada SKU.</span><span class="sxs-lookup"><span data-stu-id="3c3cf-113">You can resize a gateway SKU within the same SKU family.</span></span> <span data-ttu-id="3c3cf-114">Například pokud máte standardní SKU, můžete změnit velikost na HighPerformance SKU.</span><span class="sxs-lookup"><span data-stu-id="3c3cf-114">For example, if you have a Standard SKU, you can resize to a HighPerformance SKU.</span></span> <span data-ttu-id="3c3cf-115">Nelze změnit velikost vašich bran VPN mezi SKU staré a nové rodiny SKU.</span><span class="sxs-lookup"><span data-stu-id="3c3cf-115">You can't resize your VPN gateways between the old SKUs and the new SKU families.</span></span> <span data-ttu-id="3c3cf-116">Například nelze přejít z standardní SKU VpnGw2 SKU.</span><span class="sxs-lookup"><span data-stu-id="3c3cf-116">For example, you can't go from a Standard SKU to a VpnGw2 SKU.</span></span> 

<span data-ttu-id="3c3cf-117">Ke změně velikosti skladová položka brány pro model nasazení classic, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="3c3cf-117">To resize a gateway SKU for the classic deployment model, use the following command:</span></span>

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

<span data-ttu-id="3c3cf-118">Ke změně velikosti skladová položka brány pro model nasazení Resource Manager, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="3c3cf-118">To resize a gateway SKU for the Resource Manager deployment model, use the following command:</span></span>

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <span data-ttu-id="3c3cf-119"><a name="migrate"></a>Migrace na nové bráně SKU</span><span class="sxs-lookup"><span data-stu-id="3c3cf-119"><a name="migrate"></a>Migrate to the new gateway SKUs</span></span>

<span data-ttu-id="3c3cf-120">Pokud pracujete s modelem nasazení Resource Manager, můžete migrovat do nového SKU brány.</span><span class="sxs-lookup"><span data-stu-id="3c3cf-120">If you are working with the Resource Manager deployment model, you can migrate to the new gateway SKUS.</span></span> <span data-ttu-id="3c3cf-121">Pokud pracujete s modelem nasazení classic, nemůže se migrovat do nového SKU a místo toho musíte dál používat starší verze SKU.</span><span class="sxs-lookup"><span data-stu-id="3c3cf-121">If you are working with the classic deployment model, you can't migrate to the new SKUs and must instead continue to use the legacy SKUs.</span></span>

[!INCLUDE [Migrate SKU](../../includes/vpn-gateway-migrate-legacy-sku-include.md)]

## <a name="next-steps"></a><span data-ttu-id="3c3cf-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3c3cf-122">Next steps</span></span>

<span data-ttu-id="3c3cf-123">Další informace o nové SKU brány najdete v tématu [SKU brány](vpn-gateway-about-vpngateways.md#gwsku).</span><span class="sxs-lookup"><span data-stu-id="3c3cf-123">For more information about the new Gateway SKUs, see [Gateway SKUs](vpn-gateway-about-vpngateways.md#gwsku).</span></span>

<span data-ttu-id="3c3cf-124">Další informace o nastavení konfigurace najdete v tématu [nastavení konfigurace o službě VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md).</span><span class="sxs-lookup"><span data-stu-id="3c3cf-124">For more information about configuration settings, see [About VPN Gateway configuration settings](vpn-gateway-about-vpn-gateway-settings.md).</span></span>