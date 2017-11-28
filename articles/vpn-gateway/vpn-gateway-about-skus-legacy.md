---
title: "SKU brány virtuální sítě Azure aaaLegacy | Microsoft Docs"
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
ms.openlocfilehash: 710417581423d2fbc62827cab7949f2e137c5996
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a><span data-ttu-id="0e662-103">Práce s SKU (starší verze SKU) brány virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="0e662-103">Working with virtual network gateway SKUs (legacy SKUs)</span></span>

<span data-ttu-id="0e662-104">Tento článek obsahuje informace o hello starší verze (starý) virtuální sítě SKU brány.</span><span class="sxs-lookup"><span data-stu-id="0e662-104">This article contains information about hello legacy (old) virtual network gateway SKUs.</span></span> <span data-ttu-id="0e662-105">starší verze Hello SKU i nadále fungovat v obou modelech nasazení pro brány sítě VPN, které již byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="0e662-105">hello legacy SKUs still work in both deployment models for VPN gateways that have already been created.</span></span> <span data-ttu-id="0e662-106">Classic brány VPN pokračovat toouse hello starší verze SKU pro existující brány i pro nové brány.</span><span class="sxs-lookup"><span data-stu-id="0e662-106">Classic VPN gateways continue toouse hello legacy SKUs, both for existing gateways, and for new gateways.</span></span> <span data-ttu-id="0e662-107">Při vytváření nového správce prostředků VPN Gateway, použijte nové SKU brány hello.</span><span class="sxs-lookup"><span data-stu-id="0e662-107">When creating new Resource Manager VPN gateways, use hello new gateway SKUs.</span></span> <span data-ttu-id="0e662-108">Informace o hello nové SKU, najdete v části [o službě VPN Gateway](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="0e662-108">For information about hello new SKUs, see [About VPN Gateway](vpn-gateway-about-vpngateways.md).</span></span>

## <span data-ttu-id="0e662-109"><a name="gwsku"></a>SKU brány</span><span class="sxs-lookup"><span data-stu-id="0e662-109"><a name="gwsku"></a>Gateway SKUs</span></span>

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

## <span data-ttu-id="0e662-110"><a name="agg"></a>Odhadovaná agregovaná propustnost podle SKU</span><span class="sxs-lookup"><span data-stu-id="0e662-110"><a name="agg"></a>Estimated aggregate throughput by SKU</span></span>

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <span data-ttu-id="0e662-111"><a name="config"></a>Podporované konfigurace podle typu SKU a síť VPN</span><span class="sxs-lookup"><span data-stu-id="0e662-111"><a name="config"></a>Supported configurations by SKU and VPN type</span></span>

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <span data-ttu-id="0e662-112"><a name="resize"></a>Změnit velikost brány (změnit a skladová položka brány)</span><span class="sxs-lookup"><span data-stu-id="0e662-112"><a name="resize"></a>Resize a gateway (change a gateway SKU)</span></span>

<span data-ttu-id="0e662-113">Můžete změnit velikost skladová položka brány v rámci hello stejná rodina SKU.</span><span class="sxs-lookup"><span data-stu-id="0e662-113">You can resize a gateway SKU within hello same SKU family.</span></span> <span data-ttu-id="0e662-114">Například pokud máte standardní SKU, můžete změnit velikost tooa HighPerformance SKU.</span><span class="sxs-lookup"><span data-stu-id="0e662-114">For example, if you have a Standard SKU, you can resize tooa HighPerformance SKU.</span></span> <span data-ttu-id="0e662-115">Nelze změnit velikost VPN mezi hello staré SKU a hello nové rodiny SKU brány.</span><span class="sxs-lookup"><span data-stu-id="0e662-115">You can't resize your VPN gateways between hello old SKUs and hello new SKU families.</span></span> <span data-ttu-id="0e662-116">Například nelze přejít ze standardní SKU tooa VpnGw2 SKU.</span><span class="sxs-lookup"><span data-stu-id="0e662-116">For example, you can't go from a Standard SKU tooa VpnGw2 SKU.</span></span> 

<span data-ttu-id="0e662-117">tooresize skladová položka brány pro model nasazení classic hello hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0e662-117">tooresize a gateway SKU for hello classic deployment model, use hello following command:</span></span>

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

<span data-ttu-id="0e662-118">tooresize skladová položka brány pro hello modelu nasazení Resource Manager, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="0e662-118">tooresize a gateway SKU for hello Resource Manager deployment model, use hello following command:</span></span>

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <span data-ttu-id="0e662-119"><a name="migrate"></a>Migrace nové SKU brány toohello</span><span class="sxs-lookup"><span data-stu-id="0e662-119"><a name="migrate"></a>Migrate toohello new gateway SKUs</span></span>

<span data-ttu-id="0e662-120">Pokud pracujete s modelem nasazení Resource Manager hello, můžete migrovat toohello SKU pro nové brány.</span><span class="sxs-lookup"><span data-stu-id="0e662-120">If you are working with hello Resource Manager deployment model, you can migrate toohello new gateway SKUS.</span></span> <span data-ttu-id="0e662-121">Pokud pracujete s modelem nasazení classic hello, nemůžete migrovat, toohello nové SKU a místo toho musí pokračovat toouse hello starší verze SKU.</span><span class="sxs-lookup"><span data-stu-id="0e662-121">If you are working with hello classic deployment model, you can't migrate toohello new SKUs and must instead continue toouse hello legacy SKUs.</span></span>

[!INCLUDE [Migrate SKU](../../includes/vpn-gateway-migrate-legacy-sku-include.md)]

## <a name="next-steps"></a><span data-ttu-id="0e662-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0e662-122">Next steps</span></span>

<span data-ttu-id="0e662-123">Další informace o hello nové SKU brány najdete v tématu [SKU brány](vpn-gateway-about-vpngateways.md#gwsku).</span><span class="sxs-lookup"><span data-stu-id="0e662-123">For more information about hello new Gateway SKUs, see [Gateway SKUs](vpn-gateway-about-vpngateways.md#gwsku).</span></span>

<span data-ttu-id="0e662-124">Další informace o nastavení konfigurace najdete v tématu [nastavení konfigurace o službě VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md).</span><span class="sxs-lookup"><span data-stu-id="0e662-124">For more information about configuration settings, see [About VPN Gateway configuration settings](vpn-gateway-about-vpn-gateway-settings.md).</span></span>