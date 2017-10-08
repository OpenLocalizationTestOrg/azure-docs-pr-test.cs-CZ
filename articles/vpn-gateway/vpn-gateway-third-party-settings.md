---
title: "Navrhované komunity třetích stran VPN nebo brány firewall nastavení zařízení v Azure VPN gateway | Microsoft Docs"
description: "Další informace o navrhované komunity třetích stran VPN nebo brány firewall zařízení nastavení brány Azure VPN."
services: vpn-gateway
documentationcenter: 
author: chadmath
manager: cshepard
editor: 
tags: azure-vpn-gateway
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/04/2017
ms.author: delhan
ms.openlocfilehash: c695d23167ddba11283f6e223769e29b18a5c7ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="community-suggested-third-party-vpn-or-firewall-device-settings-for-azure-vpn-gateway"></a><span data-ttu-id="862d3-103">Navrhované komunity třetích stran VPN nebo brány firewall nastavení zařízení pro bránu Azure VPN</span><span class="sxs-lookup"><span data-stu-id="862d3-103">Community-suggested third-party VPN or firewall device settings for Azure VPN gateway</span></span>

<span data-ttu-id="862d3-104">Tento článek obsahuje několik navrhovanými řešeními pro zařízení brány firewall, které se používají službou Azure VPN gateway nebo VPN jiných výrobců.</span><span class="sxs-lookup"><span data-stu-id="862d3-104">This article provides several suggested solutions for third-party VPN or firewall devices that are used with Azure VPN gateway.</span></span>

> [!Note]
> <span data-ttu-id="862d3-105">Technickou podporu pro zařízení VPN nebo brány firewall třetích stran je poskytován dodavatelem hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="862d3-105">Technical support for third-party VPN or firewall devices is provided by hello device vendor.</span></span> 

## <a name="more-information"></a><span data-ttu-id="862d3-106">Další informace</span><span class="sxs-lookup"><span data-stu-id="862d3-106">More information</span></span>

<span data-ttu-id="862d3-107">Hello následující tabulka uvádí několik běžných zařízení a související nápovědy:</span><span class="sxs-lookup"><span data-stu-id="862d3-107">hello following table lists several common devices and related help:</span></span>

|<span data-ttu-id="862d3-108">Produkt</span><span class="sxs-lookup"><span data-stu-id="862d3-108">Product</span></span>    |<span data-ttu-id="862d3-109">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="862d3-109">Reference</span></span>                                                |
|-----------|-----------------------------------------------------------|
|<span data-ttu-id="862d3-110">Cisco ASA</span><span class="sxs-lookup"><span data-stu-id="862d3-110">Cisco ASA</span></span>  |[<span data-ttu-id="862d3-111">Komunita navrhované řešení pro Cisco ASA na Azure VPN</span><span class="sxs-lookup"><span data-stu-id="862d3-111">Community suggested solutions for Cisco ASA on Azure VPN</span></span>](https://search.cisco.com/search?query=%22Azure%20VPN%22%20ASA&locale=enUS&tab=Cisco)   |
|<span data-ttu-id="862d3-112">Cisco ISR</span><span class="sxs-lookup"><span data-stu-id="862d3-112">Cisco ISR</span></span>  |[<span data-ttu-id="862d3-113">Komunita navrhované řešení pro Cisco ISR na Azure VPN</span><span class="sxs-lookup"><span data-stu-id="862d3-113">Community suggested solutions for Cisco ISR on Azure VPN</span></span>](https://search.cisco.com/search?query=%22Azure%20VPN%22%20ISR&locale=enUS&tab=Cisco)   |
|<span data-ttu-id="862d3-114">Cisco automatické obnovení systému</span><span class="sxs-lookup"><span data-stu-id="862d3-114">Cisco ASR</span></span>  |[<span data-ttu-id="862d3-115">Komunita navrhované řešení pro automatické obnovení systému Cisco na Azure VPN</span><span class="sxs-lookup"><span data-stu-id="862d3-115">Community suggested solutions for Cisco ASR on Azure VPN</span></span>](https://search.cisco.com/search?query=%22Azure%20VPN%22%20ASR&locale=enUS&tab=Cisco)   |
|<span data-ttu-id="862d3-116">SonicWALL</span><span class="sxs-lookup"><span data-stu-id="862d3-116">Sonicwall</span></span> |<span data-ttu-id="862d3-117">Vyhledejte **Azure VPN** na [Sonicwall lokality](https://support.sonicwall.com/search)</span><span class="sxs-lookup"><span data-stu-id="862d3-117">Search for **Azure VPN** on [Sonicwall site](https://support.sonicwall.com/search)</span></span> |
| <span data-ttu-id="862d3-118">Kontrolní bod</span><span class="sxs-lookup"><span data-stu-id="862d3-118">Checkpoint</span></span>    |<span data-ttu-id="862d3-119">Vyhledejte **Azure VPN** na [kontrolního bodu lokality](https://supportcenter.checkpoint.com/supportcenter/portal)</span><span class="sxs-lookup"><span data-stu-id="862d3-119">Search for **Azure VPN** on [Checkpoint site](https://supportcenter.checkpoint.com/supportcenter/portal)</span></span> |
|<span data-ttu-id="862d3-120">Juniper</span><span class="sxs-lookup"><span data-stu-id="862d3-120">Juniper</span></span> |<span data-ttu-id="862d3-121">Vyhledejte **Azure VPN** na [Juniper lokality]( http://www.juniper.net/search/public/)</span><span class="sxs-lookup"><span data-stu-id="862d3-121">Search for **Azure VPN** on [Juniper site]( http://www.juniper.net/search/public/)</span></span>|
|<span data-ttu-id="862d3-122">Barracuda</span><span class="sxs-lookup"><span data-stu-id="862d3-122">Barracuda</span></span>  |[<span data-ttu-id="862d3-123">Komunita navrhované řešení pro Barracuda na Azure VPN</span><span class="sxs-lookup"><span data-stu-id="862d3-123">Community suggested solutions for Barracuda on Azure VPN</span></span>](https://campus.barracuda.com/search/?q=%22Azure+VPN%22&x=0&y=0)   |
|<span data-ttu-id="862d3-124">F5</span><span class="sxs-lookup"><span data-stu-id="862d3-124">F5</span></span>         |[<span data-ttu-id="862d3-125">Komunita navrhované řešení pro F5 na Azure VPN</span><span class="sxs-lookup"><span data-stu-id="862d3-125">Community suggested solutions for F5 on Azure VPN</span></span>](https://support.f5.com/csp/#/federated-search?q=%22Azure%20VPN%22&source=support)          |
|<span data-ttu-id="862d3-126">Palo</span><span class="sxs-lookup"><span data-stu-id="862d3-126">Palo</span></span>       |[<span data-ttu-id="862d3-127">Komunita navrhované řešení pro Palo na Azure VPN</span><span class="sxs-lookup"><span data-stu-id="862d3-127">Community suggested solutions for Palo on Azure VPN</span></span>](https://live.paloaltonetworks.com/t5/forums/searchpage/tab/message?q=Azure+VPN)        |
|<span data-ttu-id="862d3-128">Watchguard</span><span class="sxs-lookup"><span data-stu-id="862d3-128">Watchguard</span></span> |[<span data-ttu-id="862d3-129">Komunita navrhované řešení pro Watchguard na Azure VPN</span><span class="sxs-lookup"><span data-stu-id="862d3-129">Community suggested solutions for Watchguard on Azure VPN</span></span>](http://watchguardsupport.force.com/SupportSearch#q=Azure%20VPN&t=All&sort=relevancy)  |

## <a name="next-step"></a><span data-ttu-id="862d3-130">Další krok</span><span class="sxs-lookup"><span data-stu-id="862d3-130">Next step</span></span>

[<span data-ttu-id="862d3-131">Nastavení brány Azure</span><span class="sxs-lookup"><span data-stu-id="862d3-131">Azure Gateways settings</span></span>](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpn-devices#a-nameipsecaipsecike-parameters)

[<span data-ttu-id="862d3-132">Známé kompatibilní zařízení</span><span class="sxs-lookup"><span data-stu-id="862d3-132">Known compatible devices</span></span>](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpn-devices#validated-vpn-devices)

