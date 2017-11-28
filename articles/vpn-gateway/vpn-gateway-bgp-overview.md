---
title: "aaaOverview protokolu BGP se službou Azure VPN Gateways | Microsoft Docs"
description: "Tento článek obsahuje přehled protokolu BGP se službou Azure VPN Gateways."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: 
ms.assetid: f8c3985c-c128-4f34-835c-0e88742bf36e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/12/2017
ms.author: yushwang
ms.openlocfilehash: ced3f77ecd791c84fb72b96447e839be3bf94846
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-bgp-with-azure-vpn-gateways"></a><span data-ttu-id="b834c-103">Přehled protokolu BGP se službou Azure VPN Gateways</span><span class="sxs-lookup"><span data-stu-id="b834c-103">Overview of BGP with Azure VPN Gateways</span></span>
<span data-ttu-id="b834c-104">Tento článek obsahuje přehled o podpoře protokolu BGP (Border Gateway Protocol) ve službě Azure VPN Gateways.</span><span class="sxs-lookup"><span data-stu-id="b834c-104">This article provides an overview of BGP (Border Gateway Protocol) support in Azure VPN Gateways.</span></span>

<span data-ttu-id="b834c-105">BGP je standardní směrovací protokol hello běžně používaný v hello Internet tooexchange směrování a dostupnosti informací mezi dvěma nebo více sítěmi.</span><span class="sxs-lookup"><span data-stu-id="b834c-105">BGP is hello standard routing protocol commonly used in hello Internet tooexchange routing and reachability information between two or more networks.</span></span> <span data-ttu-id="b834c-106">Kdy použít v kontextu hello virtuálních sítí Azure, protokol BGP povolí hello Azure VPN Gateway a vaše místní zařízení VPN, názvem partnerské uzly protokolu BGP nebo Sousedé BGP, tooexchange "tras", bude informovat obě brány o dostupnosti hello a dostupnosti pro ty předpony toogo prostřednictvím hello bránami nebo trasami související se situací.</span><span class="sxs-lookup"><span data-stu-id="b834c-106">When used in hello context of Azure Virtual Networks, BGP enables hello Azure VPN Gateways and your on-premises VPN devices, called BGP peers or neighbors, tooexchange "routes" that will inform both gateways on hello availability and reachability for those prefixes toogo through hello gateways or routers involved.</span></span> <span data-ttu-id="b834c-107">Protokol BGP můžete také povolit směrování přenosu mezi více sítěmi pomocí šíření tras, které brána s protokolem BGP zjistí od jednoho tooall partnera BGP ostatním partnerům BGP.</span><span class="sxs-lookup"><span data-stu-id="b834c-107">BGP can also enable transit routing among multiple networks by propagating routes a BGP gateway learns from one BGP peer tooall other BGP peers.</span></span> 

## <a name="why-use-bgp"></a><span data-ttu-id="b834c-108">Proč používat protokol BGP?</span><span class="sxs-lookup"><span data-stu-id="b834c-108">Why use BGP?</span></span>
<span data-ttu-id="b834c-109">Protokol BGP je volitelná funkce, kterou můžete použít s trasovými bránami Azure VPN.</span><span class="sxs-lookup"><span data-stu-id="b834c-109">BGP is an optional feature you can use with Azure Route-Based VPN gateways.</span></span> <span data-ttu-id="b834c-110">Můžete také ujistěte se, že vaše místní zařízení VPN podporuje protokol BGP před povolením funkce hello.</span><span class="sxs-lookup"><span data-stu-id="b834c-110">You should also make sure your on-premises VPN devices support BGP before you enable hello feature.</span></span> <span data-ttu-id="b834c-111">Můžete pokračovat v toouse Azure VPN Gateway a vaše místní zařízení VPN bez protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="b834c-111">You can continue toouse Azure VPN gateways and your on-premises VPN devices without BGP.</span></span> <span data-ttu-id="b834c-112">Je hello ekvivalentní používání statických tras (bez protokolu BGP) *oproti* používání dynamického směrování s protokolem BGP mezi vaší sítí a Azure.</span><span class="sxs-lookup"><span data-stu-id="b834c-112">It is hello equivalent of using static routes (without BGP) *vs.* using dynamic routing with BGP between your networks and Azure.</span></span>

<span data-ttu-id="b834c-113">Existuje několik výhod a nových schopností při použití protokolu BGP:</span><span class="sxs-lookup"><span data-stu-id="b834c-113">There are several advantages and new capabilities with BGP:</span></span>

### <a name="support-automatic-and-flexible-prefix-updates"></a><span data-ttu-id="b834c-114">Podpora automatických a flexibilních aktualizací předpon</span><span class="sxs-lookup"><span data-stu-id="b834c-114">Support automatic and flexible prefix updates</span></span>
<span data-ttu-id="b834c-115">Při použití protokolu BGP musíte pouze toodeclare minimální předponu tooa konkrétní partnerský uzel protokolu BGP přes tunel IPsec S2S VPN hello.</span><span class="sxs-lookup"><span data-stu-id="b834c-115">With BGP, you only need toodeclare a minimum prefix tooa specific BGP peer over hello IPsec S2S VPN tunnel.</span></span> <span data-ttu-id="b834c-116">Může být malá jako předpona hostitele (/ 32) z hello IP adresy partnerského uzlu protokolu BGP vašeho místního zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="b834c-116">It can be as small as a host prefix (/32) of hello BGP peer IP address of your on-premises VPN device.</span></span> <span data-ttu-id="b834c-117">Můžete ovládat, který místní předpony sítě chcete tooadvertise tooAzure tooallow tooaccess vaší virtuální síti Azure.</span><span class="sxs-lookup"><span data-stu-id="b834c-117">You can control which on-premises network prefixes you want tooadvertise tooAzure tooallow your Azure Virtual Network tooaccess.</span></span>

<span data-ttu-id="b834c-118">Můžete také inzerovat delší předpony, které mohou obsahovat některé předpony adres vaší virtuální sítě, například velký prostor privátní IP adresy (např. 10.0.0.0/8).</span><span class="sxs-lookup"><span data-stu-id="b834c-118">You can also advertise larger prefixes that may include some of your VNet address prefixes, such as a large private IP address space (for example, 10.0.0.0/8).</span></span> <span data-ttu-id="b834c-119">Poznámka: když hello předpony nesmí shodovat s žádnými předponami vaší virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="b834c-119">Note though hello prefixes cannot be identical with any one of your VNet prefixes.</span></span> <span data-ttu-id="b834c-120">Tyto předpony trasy identické tooyour virtuální sítě budou odmítnuty.</span><span class="sxs-lookup"><span data-stu-id="b834c-120">Those routes identical tooyour VNet prefixes will be rejected.</span></span>

### <a name="support-multiple-tunnels-between-a-vnet-and-an-on-premises-site-with-automatic-failover-based-on-bgp"></a><span data-ttu-id="b834c-121">Podpora více tunelů mezi virtuální sítí a místním webem s automatickým převzetím služeb při selhání na základě protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="b834c-121">Support multiple tunnels between a VNet and an on-premises site with automatic failover based on BGP</span></span>
<span data-ttu-id="b834c-122">Můžete vytvořit více připojení mezi virtuální sítě Azure a vaše místní zařízení VPN v hello stejné umístění.</span><span class="sxs-lookup"><span data-stu-id="b834c-122">You can establish multiple connections between your Azure VNet and your on-premises VPN devices in hello same location.</span></span> <span data-ttu-id="b834c-123">Tato schopnost poskytuje více tunelů (cest) mezi dvěma sítěmi hello v konfiguraci aktivní aktivní.</span><span class="sxs-lookup"><span data-stu-id="b834c-123">This capability provides multiple tunnels (paths) between hello two networks in an active-active configuration.</span></span> <span data-ttu-id="b834c-124">Pokud jeden z hello tunely není připojen, hello odpovídající trasy se odvolají přes protokol BGP a provoz hello automaticky posune toohello zbývající tunely.</span><span class="sxs-lookup"><span data-stu-id="b834c-124">If one of hello tunnels is disconnected, hello corresponding routes will be withdrawn via BGP and hello traffic automatically shifts toohello remaining tunnels.</span></span>

<span data-ttu-id="b834c-125">Hello následující diagram ukazuje jednoduchý příklad tohoto vysoce dostupného nastavení:</span><span class="sxs-lookup"><span data-stu-id="b834c-125">hello following diagram shows a simple example of this highly available setup:</span></span>

![Více aktivních cest](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

### <a name="support-transit-routing-between-your-on-premises-networks-and-multiple-azure-vnets"></a><span data-ttu-id="b834c-127">Podpora směrování přenosu mezi místními sítěmi a více virtuálními sítěmi Azure</span><span class="sxs-lookup"><span data-stu-id="b834c-127">Support transit routing between your on-premises networks and multiple Azure VNets</span></span>
<span data-ttu-id="b834c-128">Protokol BGP umožňuje více bran toolearn a šířit předpony z různých sítí, zda jsou přímo nebo nepřímo připojeny.</span><span class="sxs-lookup"><span data-stu-id="b834c-128">BGP enables multiple gateways toolearn and propagate prefixes from different networks, whether they are directly or indirectly connected.</span></span> <span data-ttu-id="b834c-129">To umožňuje směrování přenosu pomocí bran Azure VPN mezi vašimi místními weby nebo napříč více službami Azure Virtual Network.</span><span class="sxs-lookup"><span data-stu-id="b834c-129">This can enable transit routing with Azure VPN gateways between your on-premises sites or across multiple Azure Virtual Networks.</span></span>

<span data-ttu-id="b834c-130">Hello následující diagram ukazuje příklad topologie vícenásobného předávání s více cestami, které mohou přenášet provoz mezi hello dvěma místními sítěmi přes brány Azure VPN v rámci Networks Microsoft hello:</span><span class="sxs-lookup"><span data-stu-id="b834c-130">hello following diagram shows an example of a multi-hop topology with multiple paths that can transit traffic between hello two on-premises networks through Azure VPN gateways within hello Microsoft Networks:</span></span>

![Vícenásobné předávání přenosu](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## <a name="bgp-faq"></a><span data-ttu-id="b834c-132">PROTOKOL BGP – NEJČASTĚJŠÍ DOTAZY</span><span class="sxs-lookup"><span data-stu-id="b834c-132">BGP FAQ</span></span>
[!INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)]

## <a name="next-steps"></a><span data-ttu-id="b834c-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b834c-133">Next steps</span></span>
<span data-ttu-id="b834c-134">V tématu [Začínáme s protokolem BGP na branách Azure VPN](vpn-gateway-bgp-resource-manager-ps.md) pro kroky tooconfigure protokolu BGP mezi různými místy a připojení VNet-to-VNet.</span><span class="sxs-lookup"><span data-stu-id="b834c-134">See [Getting started with BGP on Azure VPN gateways](vpn-gateway-bgp-resource-manager-ps.md) for steps tooconfigure BGP for your cross-premises and VNet-to-VNet connections.</span></span>

