---
title: "O brány virtuální sítě ExpressRoute | Microsoft Docs"
description: "Další informace o brány virtuální sítě pro ExpressRoute."
services: expressroute
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
tags: azure-resource-manager, azure-service-management
ms.assetid: 7e0d9658-bc00-45b0-848f-f7a6da648635
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: cherylmc
ms.openlocfilehash: a6363fa380d0bab05d7500141cc6019d1d3f68b8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="about-virtual-network-gateways-for-expressroute"></a><span data-ttu-id="76052-103">O branách virtuálních sítí pro ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="76052-103">About virtual network gateways for ExpressRoute</span></span>
<span data-ttu-id="76052-104">Brána virtuální sítě se používá k posílání síťového provozu mezi virtuálními sítěmi Azure a místními umístěními.</span><span class="sxs-lookup"><span data-stu-id="76052-104">A virtual network gateway is used to send network traffic between Azure virtual networks and on-premises locations.</span></span> <span data-ttu-id="76052-105">Při konfiguraci připojení ExpressRoute, musíte vytvořit a konfigurovat bránu virtuální sítě a připojení brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="76052-105">When you configure an ExpressRoute connection, you must create and configure a virtual network gateway and a virtual network gateway connection.</span></span>

<span data-ttu-id="76052-106">Při vytváření brány virtuální sítě je třeba zadat několik nastavení.</span><span class="sxs-lookup"><span data-stu-id="76052-106">When you create a virtual network gateway, you specify several settings.</span></span> <span data-ttu-id="76052-107">Jeden z požadovaných nastavení určuje, zda brány se použije pro provoz ExpressRoute nebo VPN typu Site-to-Site.</span><span class="sxs-lookup"><span data-stu-id="76052-107">One of the required settings specifies whether the gateway will be used for ExpressRoute or Site-to-Site VPN traffic.</span></span> <span data-ttu-id="76052-108">V modelu nasazení Resource Manager, je nastavení '-GatewayType'.</span><span class="sxs-lookup"><span data-stu-id="76052-108">In the Resource Manager deployment model, the setting is '-GatewayType'.</span></span>

<span data-ttu-id="76052-109">Pokud síťový provoz se odesílají na privátní připojení, je použít typ brány, ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="76052-109">When network traffic is sent on a private connection, you use the gateway type 'ExpressRoute'.</span></span> <span data-ttu-id="76052-110">Ten se označuje také jako brána ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="76052-110">This is also referred to as an ExpressRoute gateway.</span></span> <span data-ttu-id="76052-111">Pokud síťový provoz posílání šifrovaných přes veřejný Internet, je použít typ brány, Vpn.</span><span class="sxs-lookup"><span data-stu-id="76052-111">When network traffic is sent encrypted across the public Internet, you use the gateway type 'Vpn'.</span></span> <span data-ttu-id="76052-112">Ten se označuje také jako brána VPN.</span><span class="sxs-lookup"><span data-stu-id="76052-112">This is referred to as a VPN gateway.</span></span> <span data-ttu-id="76052-113">Připojení typu Site-to-Site, Point-to-Site a VNet-to-VNet používají bránu VPN.</span><span class="sxs-lookup"><span data-stu-id="76052-113">Site-to-Site, Point-to-Site, and VNet-to-VNet connections all use a VPN gateway.</span></span>

<span data-ttu-id="76052-114">Každá virtuální síť může mít pouze jednu bránu virtuální sítě pro každý typ brány.</span><span class="sxs-lookup"><span data-stu-id="76052-114">Each virtual network can have only one virtual network gateway per gateway type.</span></span> <span data-ttu-id="76052-115">Například můžete mít jednu bránu virtuální sítě, která má typ -GatewayType Vpn a jednu, který má typ -GatewayType ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="76052-115">For example, you can have one virtual network gateway that uses -GatewayType Vpn, and one that uses -GatewayType ExpressRoute.</span></span> <span data-ttu-id="76052-116">Tento článek se zaměřuje na bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="76052-116">This article focuses on the ExpressRoute virtual network gateway.</span></span>

## <span data-ttu-id="76052-117"><a name="gwsku"></a>SKU brány</span><span class="sxs-lookup"><span data-stu-id="76052-117"><a name="gwsku"></a>Gateway SKUs</span></span>
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

<span data-ttu-id="76052-118">Pokud chcete upgradovat bránu výkonnější skladová položka brány, ve většině případů, můžete použít rutinu prostředí PowerShell, změny velikosti-AzureRmVirtualNetworkGateway'.</span><span class="sxs-lookup"><span data-stu-id="76052-118">If you want to upgrade your gateway to a more powerful gateway SKU, in most cases you can use the 'Resize-AzureRmVirtualNetworkGateway' PowerShell cmdlet.</span></span> <span data-ttu-id="76052-119">To bude fungovat při upgradu Standard a HighPerformance SKU.</span><span class="sxs-lookup"><span data-stu-id="76052-119">This will work for upgrades to Standard and HighPerformance SKUs.</span></span> <span data-ttu-id="76052-120">Chcete-li upgradovat na UltraPerformance SKU, musíte ale bránu znovu vytvořte.</span><span class="sxs-lookup"><span data-stu-id="76052-120">However, to upgrade to the UltraPerformance SKU, you will need to recreate the gateway.</span></span>

### <span data-ttu-id="76052-121"><a name="aggthroughput"></a>Odhadovaná agregovaná propustnost podle skladová položka brány</span><span class="sxs-lookup"><span data-stu-id="76052-121"><a name="aggthroughput"></a>Estimated aggregate throughput by gateway SKU</span></span>
<span data-ttu-id="76052-122">Následující tabulka ukazuje typy brány a odhadovanou agregovanou propustnost.</span><span class="sxs-lookup"><span data-stu-id="76052-122">The following table shows the gateway types and the estimated aggregate throughput.</span></span> <span data-ttu-id="76052-123">Tato tabulka platí pro model nasazení Resource Manager i pro klasický model.</span><span class="sxs-lookup"><span data-stu-id="76052-123">This table applies to both the Resource Manager and classic deployment models.</span></span>

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="76052-124">Propustnost aplikace závisí na několika faktorech, například na začátku do konce latenci a počet tok přenosů dat, které se aplikace spustí.</span><span class="sxs-lookup"><span data-stu-id="76052-124">Application throughput depends on multiple factors, such as the end-to-end latency, and the number of traffic flows the application opens.</span></span> <span data-ttu-id="76052-125">Čísla v tabulce představují horní limit theorectically může aplikace dosáhnout v prostředí ideální.</span><span class="sxs-lookup"><span data-stu-id="76052-125">The numbers in the table represent the upper limit that the application can theorectically achieve in an ideal environment.</span></span> 
> 
>

## <span data-ttu-id="76052-126"><a name="resources"></a>Rutiny prostředí PowerShell a rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="76052-126"><a name="resources"></a>REST APIs and PowerShell cmdlets</span></span>
<span data-ttu-id="76052-127">Další zdroje technických informací a specifickou syntaxi požadavky při použití rozhraní REST API a rutin prostředí PowerShell pro konfiguraci brány virtuální sítě najdete na následujících stránkách:</span><span class="sxs-lookup"><span data-stu-id="76052-127">For additional technical resources and specific syntax requirements when using REST APIs and PowerShell cmdlets for virtual network gateway configurations, see the following pages:</span></span>

| <span data-ttu-id="76052-128">**Classic**</span><span class="sxs-lookup"><span data-stu-id="76052-128">**Classic**</span></span> | <span data-ttu-id="76052-129">**Resource Manager**</span><span class="sxs-lookup"><span data-stu-id="76052-129">**Resource Manager**</span></span> |
| --- | --- |
| [<span data-ttu-id="76052-130">PowerShell</span><span class="sxs-lookup"><span data-stu-id="76052-130">PowerShell</span></span>](https://msdn.microsoft.com/library/mt270335.aspx) |[<span data-ttu-id="76052-131">PowerShell</span><span class="sxs-lookup"><span data-stu-id="76052-131">PowerShell</span></span>](https://msdn.microsoft.com/library/mt163510.aspx) |
| [<span data-ttu-id="76052-132">REST API</span><span class="sxs-lookup"><span data-stu-id="76052-132">REST API</span></span>](https://msdn.microsoft.com/library/jj154113.aspx) |[<span data-ttu-id="76052-133">REST API</span><span class="sxs-lookup"><span data-stu-id="76052-133">REST API</span></span>](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a><span data-ttu-id="76052-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="76052-134">Next steps</span></span>
<span data-ttu-id="76052-135">V tématu [přehled ExpressRoute](expressroute-introduction.md) pro další informace o konfiguracích dostupné připojení.</span><span class="sxs-lookup"><span data-stu-id="76052-135">See [ExpressRoute Overview](expressroute-introduction.md) for more information about available connection configurations.</span></span> 

