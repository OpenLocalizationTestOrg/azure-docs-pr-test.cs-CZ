---
title: "brány virtuální sítě aaaAbout ExpressRoute | Microsoft Docs"
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
ms.openlocfilehash: 4daf4f96b4fadb00683d8e536e51734853008c50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="about-virtual-network-gateways-for-expressroute"></a><span data-ttu-id="eae83-103">O branách virtuálních sítí pro ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="eae83-103">About virtual network gateways for ExpressRoute</span></span>
<span data-ttu-id="eae83-104">Brána virtuální sítě se používá toosend síťový provoz mezi virtuálními sítěmi Azure a místní umístění.</span><span class="sxs-lookup"><span data-stu-id="eae83-104">A virtual network gateway is used toosend network traffic between Azure virtual networks and on-premises locations.</span></span> <span data-ttu-id="eae83-105">Při konfiguraci připojení ExpressRoute, musíte vytvořit a konfigurovat bránu virtuální sítě a připojení brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="eae83-105">When you configure an ExpressRoute connection, you must create and configure a virtual network gateway and a virtual network gateway connection.</span></span>

<span data-ttu-id="eae83-106">Při vytváření brány virtuální sítě je třeba zadat několik nastavení.</span><span class="sxs-lookup"><span data-stu-id="eae83-106">When you create a virtual network gateway, you specify several settings.</span></span> <span data-ttu-id="eae83-107">Jedno z hello požadované nastavení určuje, zda hello brány se použije pro provoz ExpressRoute nebo VPN typu Site-to-Site.</span><span class="sxs-lookup"><span data-stu-id="eae83-107">One of hello required settings specifies whether hello gateway will be used for ExpressRoute or Site-to-Site VPN traffic.</span></span> <span data-ttu-id="eae83-108">V modelu nasazení Resource Manager hello nastavení hello je '-GatewayType'.</span><span class="sxs-lookup"><span data-stu-id="eae83-108">In hello Resource Manager deployment model, hello setting is '-GatewayType'.</span></span>

<span data-ttu-id="eae83-109">Odeslání síťový provoz na privátní připojení používáte typ brány hello 'ExpressRoute'.</span><span class="sxs-lookup"><span data-stu-id="eae83-109">When network traffic is sent on a private connection, you use hello gateway type 'ExpressRoute'.</span></span> <span data-ttu-id="eae83-110">Toto je také odkazované tooas bránu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="eae83-110">This is also referred tooas an ExpressRoute gateway.</span></span> <span data-ttu-id="eae83-111">Při odeslání síťový provoz mezi šifrované hello veřejného Internetu, použijte typ brány hello 'Vpn'.</span><span class="sxs-lookup"><span data-stu-id="eae83-111">When network traffic is sent encrypted across hello public Internet, you use hello gateway type 'Vpn'.</span></span> <span data-ttu-id="eae83-112">Toto je odkazované tooas brána sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="eae83-112">This is referred tooas a VPN gateway.</span></span> <span data-ttu-id="eae83-113">Připojení typu Site-to-Site, Point-to-Site a VNet-to-VNet používají bránu VPN.</span><span class="sxs-lookup"><span data-stu-id="eae83-113">Site-to-Site, Point-to-Site, and VNet-to-VNet connections all use a VPN gateway.</span></span>

<span data-ttu-id="eae83-114">Každá virtuální síť může mít pouze jednu bránu virtuální sítě pro každý typ brány.</span><span class="sxs-lookup"><span data-stu-id="eae83-114">Each virtual network can have only one virtual network gateway per gateway type.</span></span> <span data-ttu-id="eae83-115">Například můžete mít jednu bránu virtuální sítě, která má typ -GatewayType Vpn a jednu, který má typ -GatewayType ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="eae83-115">For example, you can have one virtual network gateway that uses -GatewayType Vpn, and one that uses -GatewayType ExpressRoute.</span></span> <span data-ttu-id="eae83-116">Tento článek se zaměřuje na bránu virtuální sítě hello ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="eae83-116">This article focuses on hello ExpressRoute virtual network gateway.</span></span>

## <span data-ttu-id="eae83-117"><a name="gwsku"></a>SKU brány</span><span class="sxs-lookup"><span data-stu-id="eae83-117"><a name="gwsku"></a>Gateway SKUs</span></span>
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

<span data-ttu-id="eae83-118">Pokud chcete tooupgrade vaší brány tooa výkonnější hello brány SKU, ve většině případů, které můžete použít rutiny prostředí PowerShell, změny velikosti-AzureRmVirtualNetworkGateway'.</span><span class="sxs-lookup"><span data-stu-id="eae83-118">If you want tooupgrade your gateway tooa more powerful gateway SKU, in most cases you can use hello 'Resize-AzureRmVirtualNetworkGateway' PowerShell cmdlet.</span></span> <span data-ttu-id="eae83-119">To bude fungovat pro upgrady tooStandard a HighPerformance SKU.</span><span class="sxs-lookup"><span data-stu-id="eae83-119">This will work for upgrades tooStandard and HighPerformance SKUs.</span></span> <span data-ttu-id="eae83-120">Ale tooupgrade toohello UltraPerformance SKU, budete potřebovat toorecreate hello brány.</span><span class="sxs-lookup"><span data-stu-id="eae83-120">However, tooupgrade toohello UltraPerformance SKU, you will need toorecreate hello gateway.</span></span>

### <span data-ttu-id="eae83-121"><a name="aggthroughput"></a>Odhadovaná agregovaná propustnost podle skladová položka brány</span><span class="sxs-lookup"><span data-stu-id="eae83-121"><a name="aggthroughput"></a>Estimated aggregate throughput by gateway SKU</span></span>
<span data-ttu-id="eae83-122">Hello následující tabulka uvádí typy hello brány a odhadovanou agregovanou propustnost hello.</span><span class="sxs-lookup"><span data-stu-id="eae83-122">hello following table shows hello gateway types and hello estimated aggregate throughput.</span></span> <span data-ttu-id="eae83-123">Tato tabulka platí tooboth hello Resource Manager a modely nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="eae83-123">This table applies tooboth hello Resource Manager and classic deployment models.</span></span>

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="eae83-124">Propustnost aplikace závisí na několika faktorech, například hello začátku do konce latence, a hello počet přenosů toků otevře aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="eae83-124">Application throughput depends on multiple factors, such as hello end-to-end latency, and hello number of traffic flows hello application opens.</span></span> <span data-ttu-id="eae83-125">Hello čísla v hello tabulce představují hello horní limit, můžete aplikace hello theorectically dosáhnout v prostředí ideální.</span><span class="sxs-lookup"><span data-stu-id="eae83-125">hello numbers in hello table represent hello upper limit that hello application can theorectically achieve in an ideal environment.</span></span> 
> 
>

## <span data-ttu-id="eae83-126"><a name="resources"></a>Rutiny prostředí PowerShell a rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="eae83-126"><a name="resources"></a>REST APIs and PowerShell cmdlets</span></span>
<span data-ttu-id="eae83-127">Další zdroje technických informací a specifickou syntaxi požadavky při použití rozhraní REST API a rutin prostředí PowerShell pro konfiguraci brány virtuální sítě najdete v tématu hello následující stránky:</span><span class="sxs-lookup"><span data-stu-id="eae83-127">For additional technical resources and specific syntax requirements when using REST APIs and PowerShell cmdlets for virtual network gateway configurations, see hello following pages:</span></span>

| <span data-ttu-id="eae83-128">**Classic**</span><span class="sxs-lookup"><span data-stu-id="eae83-128">**Classic**</span></span> | <span data-ttu-id="eae83-129">**Resource Manager**</span><span class="sxs-lookup"><span data-stu-id="eae83-129">**Resource Manager**</span></span> |
| --- | --- |
| [<span data-ttu-id="eae83-130">PowerShell</span><span class="sxs-lookup"><span data-stu-id="eae83-130">PowerShell</span></span>](https://msdn.microsoft.com/library/mt270335.aspx) |[<span data-ttu-id="eae83-131">PowerShell</span><span class="sxs-lookup"><span data-stu-id="eae83-131">PowerShell</span></span>](https://msdn.microsoft.com/library/mt163510.aspx) |
| [<span data-ttu-id="eae83-132">REST API</span><span class="sxs-lookup"><span data-stu-id="eae83-132">REST API</span></span>](https://msdn.microsoft.com/library/jj154113.aspx) |[<span data-ttu-id="eae83-133">REST API</span><span class="sxs-lookup"><span data-stu-id="eae83-133">REST API</span></span>](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a><span data-ttu-id="eae83-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="eae83-134">Next steps</span></span>
<span data-ttu-id="eae83-135">V tématu [přehled ExpressRoute](expressroute-introduction.md) pro další informace o konfiguracích dostupné připojení.</span><span class="sxs-lookup"><span data-stu-id="eae83-135">See [ExpressRoute Overview](expressroute-introduction.md) for more information about available connection configurations.</span></span> 

