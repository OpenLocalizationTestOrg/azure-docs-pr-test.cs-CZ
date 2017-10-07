---
title: "aaaQoS požadavky pro ExpressRoute | Microsoft Docs"
description: "Tato stránka obsahuje podrobné požadavky pro konfiguraci a správu technologie QoS pro okruhy ExpressRoute."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: db1c1447-0283-4a09-907b-ae481adc40c7
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: cherylmc
ms.openlocfilehash: 46cc81bd38ff50dd9e7a1bfdd0faa457ff7b2fa1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-qos-requirements"></a><span data-ttu-id="812bb-103">Požadavky na technologii QoS služby ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="812bb-103">ExpressRoute QoS requirements</span></span>
<span data-ttu-id="812bb-104">Skype pro firmy má úlohy, které vyžadují odlišné zacházení podle QoS.</span><span class="sxs-lookup"><span data-stu-id="812bb-104">Skype for Business has various workloads that require differentiated QoS treatment.</span></span> <span data-ttu-id="812bb-105">Pokud máte v plánu tooconsume hlasové služby prostřednictvím ExpressRoute, měli byste dodržovat toohello požadavky popsané dál.</span><span class="sxs-lookup"><span data-stu-id="812bb-105">If you plan tooconsume voice services through ExpressRoute, you should adhere toohello requirements described below.</span></span>

![](./media/expressroute-qos/expressroute-qos.png)

> [!NOTE]
> <span data-ttu-id="812bb-106">Požadavky na technologii QoS použít toohello Microsoft jenom partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="812bb-106">QoS requirements apply toohello Microsoft peering only.</span></span> <span data-ttu-id="812bb-107">hodnoty DSCP Hello v provozu sítě byla přijata veřejný partnerský vztah Azure a soukromý partnerský vztah Azure budou too0 resetování.</span><span class="sxs-lookup"><span data-stu-id="812bb-107">hello DSCP values in your network traffic received on Azure public peering and Azure private peering will be reset too0.</span></span> 
> 
> 

<span data-ttu-id="812bb-108">Hello následující tabulka obsahuje seznam označení DSCP používaných Skypem pro firmy.</span><span class="sxs-lookup"><span data-stu-id="812bb-108">hello following table provides a list of DSCP markings used by Skype for Business.</span></span> <span data-ttu-id="812bb-109">Odkazovat příliš[Správa technologie QoS pro Skype pro firmy](https://technet.microsoft.com/library/gg405409.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="812bb-109">Refer too[Managing QoS for Skype for Business](https://technet.microsoft.com/library/gg405409.aspx) for more information.</span></span>

| <span data-ttu-id="812bb-110">**Třída provozu**</span><span class="sxs-lookup"><span data-stu-id="812bb-110">**Traffic Class**</span></span> | <span data-ttu-id="812bb-111">**Zpracování (označení DSCP)**</span><span class="sxs-lookup"><span data-stu-id="812bb-111">**Treatment (DSCP Marking)**</span></span> | <span data-ttu-id="812bb-112">**Úlohy Skypu pro firmy**</span><span class="sxs-lookup"><span data-stu-id="812bb-112">**Skype for Business Workloads**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="812bb-113">**Hlas**</span><span class="sxs-lookup"><span data-stu-id="812bb-113">**Voice**</span></span> |<span data-ttu-id="812bb-114">EF (46)</span><span class="sxs-lookup"><span data-stu-id="812bb-114">EF (46)</span></span> |<span data-ttu-id="812bb-115">Skype/Lync – hlas</span><span class="sxs-lookup"><span data-stu-id="812bb-115">Skype / Lync voice</span></span> |
| <span data-ttu-id="812bb-116">**Interaktivní**</span><span class="sxs-lookup"><span data-stu-id="812bb-116">**Interactive**</span></span> |<span data-ttu-id="812bb-117">AF41 (34)</span><span class="sxs-lookup"><span data-stu-id="812bb-117">AF41 (34)</span></span> |<span data-ttu-id="812bb-118">Video, VBSS</span><span class="sxs-lookup"><span data-stu-id="812bb-118">Video, VBSS</span></span> |
| <span data-ttu-id="812bb-119">AF21 (18)</span><span class="sxs-lookup"><span data-stu-id="812bb-119">AF21 (18)</span></span> |<span data-ttu-id="812bb-120">Sdílení aplikací</span><span class="sxs-lookup"><span data-stu-id="812bb-120">App sharing</span></span> | |
| <span data-ttu-id="812bb-121">**Výchozí**</span><span class="sxs-lookup"><span data-stu-id="812bb-121">**Default**</span></span> |<span data-ttu-id="812bb-122">AF11 (10)</span><span class="sxs-lookup"><span data-stu-id="812bb-122">AF11 (10)</span></span> |<span data-ttu-id="812bb-123">Přenos souborů</span><span class="sxs-lookup"><span data-stu-id="812bb-123">File transfer</span></span> |
| <span data-ttu-id="812bb-124">CS0 (0)</span><span class="sxs-lookup"><span data-stu-id="812bb-124">CS0 (0)</span></span> |<span data-ttu-id="812bb-125">Cokoliv jiného</span><span class="sxs-lookup"><span data-stu-id="812bb-125">Anything else</span></span> | |

* <span data-ttu-id="812bb-126">Měli byste klasifikovat úlohy hello a označit správné hodnoty DSCP hello.</span><span class="sxs-lookup"><span data-stu-id="812bb-126">You should classify hello workloads and mark hello right DSCP values.</span></span> <span data-ttu-id="812bb-127">Postupujte podle pokynů hello uvedených [sem](https://technet.microsoft.com/library/gg405409.aspx) o označení DSCP tooset ve vaší síti.</span><span class="sxs-lookup"><span data-stu-id="812bb-127">Follow hello guidance provided [here](https://technet.microsoft.com/library/gg405409.aspx) on how tooset DSCP markings in your network.</span></span>
* <span data-ttu-id="812bb-128">Měli byste ve vaší síti nakonfigurovat a podporovat více front QoS.</span><span class="sxs-lookup"><span data-stu-id="812bb-128">You should configure and support multiple QoS queues within your network.</span></span> <span data-ttu-id="812bb-129">Hlas musí být samostatná třída a přijímat zpracování EF hello uvedené v dokumentu RFC 3246.</span><span class="sxs-lookup"><span data-stu-id="812bb-129">Voice must be a standalone class and receive hello EF treatment specified in RFC 3246.</span></span> 
* <span data-ttu-id="812bb-130">Můžete rozhodnout hello služby Řízení front mechanismus, zásadách detekce zahlcení a přidělení šířky pásma jednotlivé třídy provozu.</span><span class="sxs-lookup"><span data-stu-id="812bb-130">You can decide hello queuing mechanism, congestion detection policy, and bandwidth allocation per traffic class.</span></span> <span data-ttu-id="812bb-131">Ale hello označení DSCP pro Skype pro firmy musí být zachováno.</span><span class="sxs-lookup"><span data-stu-id="812bb-131">But, hello DSCP marking for Skype for Business workloads must be preserved.</span></span> <span data-ttu-id="812bb-132">Pokud používáte označení DSCP není uvedené výš, například AF31 (26), je třeba tento too0 hodnotu DSCP přepsat před odesláním paketu tooMicrosoft hello.</span><span class="sxs-lookup"><span data-stu-id="812bb-132">If you are using DSCP markings not listed above, e.g. AF31 (26), you must rewrite this DSCP value too0 before sending hello packet tooMicrosoft.</span></span> <span data-ttu-id="812bb-133">Microsoft odesílá jenom pakety označené hodnotami DSCP uvedenými v hello výše tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="812bb-133">Microsoft only sends packets marked with hello DSCP value shown in hello above table.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="812bb-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="812bb-134">Next steps</span></span>
* <span data-ttu-id="812bb-135">Odkazovat toohello požadavky pro [směrování](expressroute-routing.md) a [NAT](expressroute-nat.md).</span><span class="sxs-lookup"><span data-stu-id="812bb-135">Refer toohello requirements for [Routing](expressroute-routing.md) and [NAT](expressroute-nat.md).</span></span>
* <span data-ttu-id="812bb-136">Zjistit hello následující odkazy tooconfigure připojení ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="812bb-136">See hello following links tooconfigure your ExpressRoute connection.</span></span>
  
  * [<span data-ttu-id="812bb-137">Vytvoření okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="812bb-137">Create an ExpressRoute circuit</span></span>](expressroute-howto-circuit-classic.md)
  * [<span data-ttu-id="812bb-138">Konfigurace směrování</span><span class="sxs-lookup"><span data-stu-id="812bb-138">Configure routing</span></span>](expressroute-howto-routing-classic.md)
  * [<span data-ttu-id="812bb-139">Propojení virtuální sítě tooan okruh ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="812bb-139">Link a VNet tooan ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-classic.md)

