---
title: "Požadavky technologie QoS pro ExpressRoute | Dokumentace Microsoftu"
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
ms.openlocfilehash: c097a9ccba91f59b323215d42d37e6d85e0981ce
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="expressroute-qos-requirements"></a><span data-ttu-id="a5722-103">Požadavky na technologii QoS služby ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="a5722-103">ExpressRoute QoS requirements</span></span>
<span data-ttu-id="a5722-104">Skype pro firmy má úlohy, které vyžadují odlišné zacházení podle QoS.</span><span class="sxs-lookup"><span data-stu-id="a5722-104">Skype for Business has various workloads that require differentiated QoS treatment.</span></span> <span data-ttu-id="a5722-105">Pokud budete chtít využívat hlasové služby prostřednictvím ExpressRoute, měli byste dodržovat požadavky popsané dál.</span><span class="sxs-lookup"><span data-stu-id="a5722-105">If you plan to consume voice services through ExpressRoute, you should adhere to the requirements described below.</span></span>

![](./media/expressroute-qos/expressroute-qos.png)

> [!NOTE]
> <span data-ttu-id="a5722-106">Požadavky na technologii QoS se týkají jenom partnerského vztahu Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="a5722-106">QoS requirements apply to the Microsoft peering only.</span></span> <span data-ttu-id="a5722-107">Hodnoty DSCP v provozu vaší sítě přijaté v rámci veřejného partnerského vztahu Azure a soukromého partnerského vztahu Azure budou vynulovány.</span><span class="sxs-lookup"><span data-stu-id="a5722-107">The DSCP values in your network traffic received on Azure public peering and Azure private peering will be reset to 0.</span></span> 
> 
> 

<span data-ttu-id="a5722-108">V následující tabulce je seznam označení DSCP používaných Skypem pro firmy.</span><span class="sxs-lookup"><span data-stu-id="a5722-108">The following table provides a list of DSCP markings used by Skype for Business.</span></span> <span data-ttu-id="a5722-109">Další informace najdete v tématu [Správa technologie QoS pro Skype pro firmy](https://technet.microsoft.com/library/gg405409.aspx).</span><span class="sxs-lookup"><span data-stu-id="a5722-109">Refer to [Managing QoS for Skype for Business](https://technet.microsoft.com/library/gg405409.aspx) for more information.</span></span>

| <span data-ttu-id="a5722-110">**Třída provozu**</span><span class="sxs-lookup"><span data-stu-id="a5722-110">**Traffic Class**</span></span> | <span data-ttu-id="a5722-111">**Zpracování (označení DSCP)**</span><span class="sxs-lookup"><span data-stu-id="a5722-111">**Treatment (DSCP Marking)**</span></span> | <span data-ttu-id="a5722-112">**Úlohy Skypu pro firmy**</span><span class="sxs-lookup"><span data-stu-id="a5722-112">**Skype for Business Workloads**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a5722-113">**Hlas**</span><span class="sxs-lookup"><span data-stu-id="a5722-113">**Voice**</span></span> |<span data-ttu-id="a5722-114">EF (46)</span><span class="sxs-lookup"><span data-stu-id="a5722-114">EF (46)</span></span> |<span data-ttu-id="a5722-115">Skype/Lync – hlas</span><span class="sxs-lookup"><span data-stu-id="a5722-115">Skype / Lync voice</span></span> |
| <span data-ttu-id="a5722-116">**Interaktivní**</span><span class="sxs-lookup"><span data-stu-id="a5722-116">**Interactive**</span></span> |<span data-ttu-id="a5722-117">AF41 (34)</span><span class="sxs-lookup"><span data-stu-id="a5722-117">AF41 (34)</span></span> |<span data-ttu-id="a5722-118">Video, VBSS</span><span class="sxs-lookup"><span data-stu-id="a5722-118">Video, VBSS</span></span> |
| <span data-ttu-id="a5722-119">AF21 (18)</span><span class="sxs-lookup"><span data-stu-id="a5722-119">AF21 (18)</span></span> |<span data-ttu-id="a5722-120">Sdílení aplikací</span><span class="sxs-lookup"><span data-stu-id="a5722-120">App sharing</span></span> | |
| <span data-ttu-id="a5722-121">**Výchozí**</span><span class="sxs-lookup"><span data-stu-id="a5722-121">**Default**</span></span> |<span data-ttu-id="a5722-122">AF11 (10)</span><span class="sxs-lookup"><span data-stu-id="a5722-122">AF11 (10)</span></span> |<span data-ttu-id="a5722-123">Přenos souborů</span><span class="sxs-lookup"><span data-stu-id="a5722-123">File transfer</span></span> |
| <span data-ttu-id="a5722-124">CS0 (0)</span><span class="sxs-lookup"><span data-stu-id="a5722-124">CS0 (0)</span></span> |<span data-ttu-id="a5722-125">Cokoliv jiného</span><span class="sxs-lookup"><span data-stu-id="a5722-125">Anything else</span></span> | |

* <span data-ttu-id="a5722-126">Měli byste klasifikovat úlohy a označit správné hodnoty DSCP.</span><span class="sxs-lookup"><span data-stu-id="a5722-126">You should classify the workloads and mark the right DSCP values.</span></span> <span data-ttu-id="a5722-127">Postupujte podle pokynů uvedených [zde](https://technet.microsoft.com/library/gg405409.aspx), kde se popisuje, jak ve vaší síti nastavit označení DSCP.</span><span class="sxs-lookup"><span data-stu-id="a5722-127">Follow the guidance provided [here](https://technet.microsoft.com/library/gg405409.aspx) on how to set DSCP markings in your network.</span></span>
* <span data-ttu-id="a5722-128">Měli byste ve vaší síti nakonfigurovat a podporovat více front QoS.</span><span class="sxs-lookup"><span data-stu-id="a5722-128">You should configure and support multiple QoS queues within your network.</span></span> <span data-ttu-id="a5722-129">Hlas musí být samostatná třída a přijímat zpracování EF uvedené v dokumentu RFC 3246.</span><span class="sxs-lookup"><span data-stu-id="a5722-129">Voice must be a standalone class and receive the EF treatment specified in RFC 3246.</span></span> 
* <span data-ttu-id="a5722-130">Můžete rozhodnout o mechanismu řízení front, zásadách detekce zahlcení a přidělení šířky pásma pro jednotlivé třídy provozu.</span><span class="sxs-lookup"><span data-stu-id="a5722-130">You can decide the queuing mechanism, congestion detection policy, and bandwidth allocation per traffic class.</span></span> <span data-ttu-id="a5722-131">Označení DSCP pro Skype pro firmy ale musí být zachováno.</span><span class="sxs-lookup"><span data-stu-id="a5722-131">But, the DSCP marking for Skype for Business workloads must be preserved.</span></span> <span data-ttu-id="a5722-132">Pokud používáte označení DSCP, které není uvedené výš, například AF31 (26), musíte před odesláním paketu Microsoftu tuto hodnotu DSCP přepsat na 0.</span><span class="sxs-lookup"><span data-stu-id="a5722-132">If you are using DSCP markings not listed above, e.g. AF31 (26), you must rewrite this DSCP value to 0 before sending the packet to Microsoft.</span></span> <span data-ttu-id="a5722-133">Microsoft odesílá jenom pakety označené hodnotami DSCP uvedenými v tabulce výš.</span><span class="sxs-lookup"><span data-stu-id="a5722-133">Microsoft only sends packets marked with the DSCP value shown in the above table.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="a5722-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a5722-134">Next steps</span></span>
* <span data-ttu-id="a5722-135">Přečtěte si požadavky pro [směrování](expressroute-routing.md) a [překlad adres (NAT)](expressroute-nat.md).</span><span class="sxs-lookup"><span data-stu-id="a5722-135">Refer to the requirements for [Routing](expressroute-routing.md) and [NAT](expressroute-nat.md).</span></span>
* <span data-ttu-id="a5722-136">Následující odkazy popisují konfiguraci připojení ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="a5722-136">See the following links to configure your ExpressRoute connection.</span></span>
  
  * [<span data-ttu-id="a5722-137">Vytvoření okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="a5722-137">Create an ExpressRoute circuit</span></span>](expressroute-howto-circuit-classic.md)
  * [<span data-ttu-id="a5722-138">Konfigurace směrování</span><span class="sxs-lookup"><span data-stu-id="a5722-138">Configure routing</span></span>](expressroute-howto-routing-classic.md)
  * [<span data-ttu-id="a5722-139">Propojení virtuální sítě s okruhem ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="a5722-139">Link a VNet to an ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-classic.md)

