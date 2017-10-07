---
title: "aaaPoint společnosti internetové domény tooa název domény Traffic Manageru | Microsoft Docs"
description: "Tento článek vám pomůže bodu domény název tooa Traffic Manager název domény vaší společnosti."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 29822946-2d45-4434-ba47-fc180a445cc3
ms.service: traffic-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: kumud
ms.openlocfilehash: 84c428f60a1dc70452bf957d98a68c95e0b51715
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="point-a-company-internet-domain-tooan-azure-traffic-manager-domain"></a><span data-ttu-id="8c1a1-103">Nasměrování společnosti internetové domény tooan Azure Traffic Manager domény</span><span class="sxs-lookup"><span data-stu-id="8c1a1-103">Point a company Internet domain tooan Azure Traffic Manager domain</span></span>

<span data-ttu-id="8c1a1-104">Když vytvoříte profil Traffic Manageru, Azure mu automaticky přiřadí název DNS.</span><span class="sxs-lookup"><span data-stu-id="8c1a1-104">When you create a Traffic Manager profile, Azure automatically assigns a DNS name for that profile.</span></span> <span data-ttu-id="8c1a1-105">toouse název ze zóny DNS vytvořit záznam CNAME DNS, který mapuje název domény toohello vašeho profilu Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="8c1a1-105">toouse a name from your DNS zone, create a CNAME DNS record that maps toohello domain name of your Traffic Manager profile.</span></span> <span data-ttu-id="8c1a1-106">Název domény Traffic Manageru hello můžete najít v hello **Obecné** části na stránce konfigurace hello hello profil služby Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="8c1a1-106">You can find hello Traffic Manager domain name in hello **General** section on hello Configuration page of hello Traffic Manager profile.</span></span>

<span data-ttu-id="8c1a1-107">Například toopoint název www.contoso.com toohello contoso.trafficmanager.net název DNS Traffic Manageru, vytvořili byste hello následující záznam prostředku DNS:</span><span class="sxs-lookup"><span data-stu-id="8c1a1-107">For example, toopoint name www.contoso.com toohello Traffic Manager DNS name contoso.trafficmanager.net, you would create hello following DNS resource record:</span></span>

    www.contoso.com IN CNAME contoso.trafficmanager.net

<span data-ttu-id="8c1a1-108">Veškeré požadavky na provoz příliš*www.contoso.com* získat směrované příliš*contoso.trafficmanager.net*.</span><span class="sxs-lookup"><span data-stu-id="8c1a1-108">All traffic requests too*www.contoso.com* get directed too*contoso.trafficmanager.net*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8c1a1-109">Nemůže odkazovat domény druhé úrovně, jako například *contoso.com*, toohello doménu Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="8c1a1-109">You cannot point a second-level domain, such as *contoso.com*, toohello Traffic Manager domain.</span></span> <span data-ttu-id="8c1a1-110">Standardy protokolu DNS nepovolují záznamy CNAME pro názvy domén druhé úrovně.</span><span class="sxs-lookup"><span data-stu-id="8c1a1-110">DNS protocol standards do not allow CNAME records for second-level domain names.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8c1a1-111">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8c1a1-111">Next steps</span></span>

* [<span data-ttu-id="8c1a1-112">Metody směrování Traffic Manageru</span><span class="sxs-lookup"><span data-stu-id="8c1a1-112">Traffic Manager routing methods</span></span>](traffic-manager-routing-methods.md)
* [<span data-ttu-id="8c1a1-113">Traffic Manager – Zakázání, povolení nebo odstranění profilu</span><span class="sxs-lookup"><span data-stu-id="8c1a1-113">Traffic Manager - Disable, enable or delete a profile</span></span>](disable-enable-or-delete-a-profile.md)
* [<span data-ttu-id="8c1a1-114">Traffic Manager – Zakázání nebo povolení koncového bodu</span><span class="sxs-lookup"><span data-stu-id="8c1a1-114">Traffic Manager - Disable or enable an endpoint</span></span>](disable-or-enable-an-endpoint.md)
